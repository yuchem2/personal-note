`object` 키워드는 [클래스](클래스.md)를 정의하는 동시에 인스턴스(객체)를 생성하기 위해 사용한다. 총 세가지 경우로 사용될 수 있다.
- 객체 선언: 싱글턴을 정의하는 한 방법
- 동반 객체: 어떤 클래스와 관련이 있지만 호출하기 위해 그 클래스의 객체가 필요하지 않는 메서드와 팩토리 메서드를 담을 때 사용
- 객체 식: 자바의 익명 내부 클래스 대신 사용
## 객체 선언
```kotlin
object Payroll {
    val allEmployess = arrayListOf<Person>()
    
    fun calculateSalary() {
        for (person in allEmployess) {
            /* ... */
        }
    }
}
```

객체 선언은 `object` 키워드로 시작한다. 클래스를 정의하고 그 클래스의 인스턴스를 만들어 변수에 저장하는 모든 작업을 한 문장으로 처리한다. 프로퍼티, 메서드, 초기호 ㅏ블록 등이 들어갈 수 있지만 생성자를 쓸 수 없다. 객체 선언문이 있는 위치에서 객체가 생성자 호출 없이 즉시 만들어지기 때문이다. 객체 선언도 클래스나 인스턴스를 상속할 수 있다.

> **코틀린 객체를 자바에서 사용하기**
> 코틀린 객체는 선언은 유일한 인스턴스에 대한 정적인 필드가 있는 자바 클래스로 컴파일된다. 이때 인스턴스 필드의 이름은 항상 `INSTANCE`이다. 자바 코드에서 코틀린 싱글턴 객체를 사용하려면 정적인 `INSTANCE` 필드를 통하면 된다.
## 동반 객체
```kotlin
class MyClass {
    companion object {
        fun callMe() {
            println("Compainon object called")
        }
    }
}

fun main() {
    MyClass.callMe()
}
```

코틀린에서는 `static` 키워드를 지원하지 않기 때문에 정적인 멤버가 없다. 하지만 정적인 멤버가 필요한 경우가 있는데, 이를 해결할 수 있는 방법이 동반 객체이다. 클래스 안에 정의된 객체 중 하나에 `compainon` 키워드를 붙이면 된다.

동반 객체는 자신에 대응하는 클래스에 속하게 된다. 따라서 해당 클래스의 인스턴스는 동반 객체의 멤버에 접근할 수 없다. 

```kotlin
class User private constructor(val nickname: String) {
    companion object {
        fun newSubscribingUser(email: String) = 
            User(email.substringBefore('@'))
        fun newSocialUser(accountId: Int) =
            User(getNameFromSocialNetwork(accountId))
    }
}
```

동반 객체는 `private`로 선언된 생성자를 호출하기 좋은 위치이다. 동반 객체는 자신을 둘러싼 클래스의 모든 `private` 멤버에 접근이 가능하기 때문이다. 위와 같이 팩토리 메서드를 사용한다면 그 팩토리 메서드가 선언된 클래스의 하위 클래스 객체를 반환할 수 도 있다. 하지만 이런 클래스를 확장해야만 하는 경우에는 동반 객체 멤버를 하위 클래스에서 오버라이드할 수 없다.

```kotlin
class Person(val name: String) {
    companion object Loader {
        fun fromJSON(jsonText: String): Person = /* ... */
    }
}

fun main() {
    val person = Person.Loader.fromJSON("...")
    val person2 = Person.fromJSON("...")
}
```

동반 객체는 클래스 안에 정의된 일반 객체이다. 따라서 객체 선언처럼 동반 객체에 이름을 붙이거나 동반 객체가 인터페이스를 사용하거나, 동반 객체 안에 확장 함수와 프로퍼티를 정의할 수 있다.

클래스에는 단 하나의 동반 객체만 존재할 수 있고, 특별히 이름을 지정하지 않으면 `Companion`이 된다. 

> **코틀린 동반 객체와 정적 멤버**
> 클래스의 동반 객체는 일반 객체와 비슷한 방시긍로, 클래스에 정의된 인스턴스를 가리키는 정적 필드로 컴파일 된다. 그러므로 이름을 정했다면 그 이름으로, 안정했다면 `Companion`로 자바에서도 그 참조에 접근이 가능하다. 
> 
> 때로 자바에서 사용하기 위해 코틀린 멤버를 정적 멤버로 사용하기 위해서는 `@JvmStatic` 어노테이션을 코틀린 멤버에 붙이면 된다. 

```kotlin
class Person(val firstName: String, val lastName: String) {
    companion object {
    }
}

fun Person.Companion.fromJSON(json: String): Person {
    /* ... */
}
```

클래스에 동반 객체가 있다면 그 객체 안에 함수를 정의함으로써 클래스에 대해 호출할 수 있는 확장 함수를 만들 수 있다. 
## 객체 식
```kotlin
interface MouseListener {
    fun onEnter()
    fun onClick()
}

class Button(private val listener: MouseListener) { /* ... */ }

fun main() {
    Button(object : MouseListener) {
        override fun onEnter() { /* ... */ }
        override fun onClick() { /* ... */ }
    })
}
```

익명 객체를 정의할 때도 `object` 키워드를 활용할 수 있다. 구문은 객체 선언과 동일하지만, 이름이 빠졌다는 점이다. 객체 식은 클래스를 정의하고 그 클래스에 속한 인스턴스를 생성하지만 그 클래스나 인스턴스에 이름을 붙이지는 않는다.

객체 식은 익명 객체 안에서 여러 메서드를 오버라이드해야 하는 경우에 더 유용하다. 메서드가 하나뿐인 인터페이스를 구현해야 한다면 SAM 변환 지원을 활용하는 경우가 더 낫다.
