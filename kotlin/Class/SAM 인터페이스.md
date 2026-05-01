## 개요

```java
public interface OnClickListener {
    void onClick(View v);
}

// 자바 8 이전
button.setOnClickListener(new OnClickListener() {
    @Override
    public void onClick(View v) { /* ... */ }
})

// 자바 8부터
button.setOnClickListener(view -> { /* ... */ })
```

추상 메서드가 하나뿐인 [인터페이스](인터페이스.md) SAM은 Single Abstract Method 인터페이스라고 한다. 함수형 인터페이스라고도 한다. 일반적으로 자바 API에서는 `Runable`, `Callable` 등의 함수형 인터페이스가 매우 많다. 

```kotlin
button.setOnClickListener { view -> /* ... */ }
button.setOnClickListener(object: OnCLickListener {
    override fun onClick(View v) { /* ... */ }
})
```

코틀린에서는 이런 SAM 인터페이스를 받는 자바 메서드를 호출할 때 [람다](람다.md)를 사용할 수 있게 해준다. 이때 컴파일러는 자동으로 람다를 해당하는 SAM 인스턴스(SAM 인터페이스를 구현하는 익명 클래스의 인스턴스)로 변환해준다. 인스턴스를 명시적으로 익명 객체를 만들어 똑같은 효과를 볼 수 있다.

```kotlin
// 전체 프로그램에 Runnalbe 인스턴스 하나만 생성됨
postponeComputation(1000) { println(42) }

// 호출때마다 새 Runnable 인스턴스가 만들어짐
fun handleComputation(id: string) {
    postponeComputation(1000) {
        println(id)
    }
}
```

> 람다에 대한 익명 클래스와 그 클래스의 인스턴스를 생성하는 것에 대한 것은 오직 SAM 인터페이스를 받는 인터페이스에 대해서만 성립한다. 코틀린 확장 함수를 사용하는 컬렉션에 대해서는 성립하지 않는다. 람다를 `inline` 표시가 되어 있는 코틀린 함수에 전달하면 익명 클래스가 생기지 않는다.
 
하지만, 명시적으로 익명 객체를 만드는 경우 매번 호출할 때마다 실제로 새 인스턴스가 생긴다. 그러나 [람다](람다.md)를 활용하는 경우 **자신이 정의된 함수의 변수에 접근하지 않는 경우 함수가 호출될 때마다 람다에 해당하는 익명 객체가 재사용된다.** 반대의 경우 각가의 호출마다 새로운 인스턴스를 만들고, 그 객체 안에 캡처한 변수를 저장한다.
## SAM 생성자
```kotlin
fun createAllDOneRunnable(): Runnable {
    return Runnable { println("All done!") }
}

fun main() {
    createAllDoneRunnable().run()
}
```

SAM 생성자는 컴파일러가 생성한 함수로 [람다](람다.md)를 SAM 인스턴스의 인스턴스로 명시적 변환을 해준다. 이를 컴파일러가 변환을 자동으로 수행하지 못하는 맥락에서 사용할 수 있다. SAM 생성자는 다음과 같은 특징을 가진다.
- SAM 생성자의 이름은 사용하려는 SAM 인스턴스의 이름과 같다.
- SAM 생성자는 하나의 인자(추상 메서드의 본문에 사용할 람다)만 받아 인스턴스를 반환

```kotlin
val listener = OnClickListener { view -> 
    val text = when (view.id) {
        button1.id -> "First button"
        button2.id -> "Seconde button"
        else -> "Unknown button"
    }
    toast(text)
}
button1.setOnClickListener(listner)
button2.setOnClickListener(listner)
```

SAM 생성자는 다음과 같은 경우에 활용할 수 있다.
- SAM 인터페이스 인스턴스 값을 반환할 때
- 람다로 생성한 SAM 인터페이스를 변수에 저장해야 하는 경우
- 오버로드한 메서드 중에서 어떤 타입의 메서드를 선택해 람다를 변환해 넘겨줄지 모호할 때
## SAM 인터페이스 정의
```kotlin
fun interface IntCondition {
    fun check(i: Int): Boolean
    fun checkString(s: String) = check(s.toInt())
    fun checkChar(c: Char) =  check(c.digitToInt())
}
```

> 코틀린에서 SAM 인터페이스는 하나의 추상 메서드만을 포함하지만, 다른 비추상 메서드를 포함할 수 있다.

코틀린에서 SAM 인터페이스를 사용해야 할 부분에서 함수 타입을 사용해 행동을 표현하는 경우가 있다. 하지만 좀 더 명시적으로 함수 타입을 정의하고 싶은 경우 `fun interface`를 사용하면 SAM 인터페이스를 정의할 수 있다.

```kotlin
fun checkCondition(i: Int, condition: IntCondition): Boolean {
    return condition.check(i)
}

fun main() {
    checkCondition(1) { it % 2 != 0 }
    val isOdd: (Int) -> Boolean = { it % 2 != 0 }
    checkCondition(1, isOdd)
}
```

`fun interface`로 정의된 타입의 파라미터를 받는 함수가 있을 때 람다 구현이나 람다에 대한 참조를 직접 넘길 수 있다. 두 경우 모두 동적으로 인터페이스 구현을 인스턴스화해준다.

> **자바 코드에서 SAM 인터페이스 호출 부분 더 깔끔하게 만들기**
> 자바와 코틀린을 함께 사용하는 코드를 작성할 때 `fun interface`를 쓰면 자바에서 함수를 호출하는 코드의 간결성이 더 높아진다. 
> 
> 코틀린 함수 타입은 파라미터와 반환 타입을 타입 파라미터로 하는 제네릭 타입인 객체로 번역된다. 아무것도 반환하지 않는 함수의 경우 `Unit`을 `void`에 해당하는 역할로 사용한다. 이러한 경우 자바에서 호출할 때 `Unit.INSTANCE`를 반환할 필요가 있다. `fun interface`를 활용하는 경우 요구사항을 피할 수 있고, 호출이 간결해진다.
> 
> ```kotlin
> fun interface StringConsumer {
>     fun consume(s: String)
> }
> 
> fun consumeHello(t: StringConsumer) {
>     t.consume("Hello")
> }
> 
> fun consumeHeeloFunctional(t: (String) -> Unit) {
>     t("Hello")
> }
> ```
> 
> ```java
> import kotlin.Unit;
> 
> public class MyApp {
>     public static void main(String[] args) {
>         // SAM 인터페이스를 활용한 경우
>         MainKt.consumeHello(s -> System.out.println(s.toUpperCase()));
>         // 함수 타입을 사용하는 경우
>         MainKt.consumeHelloFunctional(s -> {
>             System.out.println(s.toUpperCase());
>             return Unit.INSTANCE;
>         });
>     }
> }
> ```

