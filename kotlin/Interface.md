```Kotlin
interface Clickable {
    fun click()
}
```

코틀린 인터페이스는 추상 메서드뿐 아니라 구현이 있는 메서드도 정의할 수 있다. 하지만, 아무 상태도 들어갈 수 없다. 이 인터페이스를 상속하는 구현하는 모든 비추상 클래스는 `click`에 대한 구현을 제공해야 한다.

```kotlin
class Button : Clickable {
    override fun click() = println("I was clicked")
}
```

코틀린에서는 **상속**이나 **구성**에서 모두 클래스 이름 뒤에 콜론을 붙이고 인터페이스나 클래스 이름을 적는 방식을 사용한다. 하지만 인터페이스는 개수 제한이 없지만, 클래스는 오직 하나만 확장할 수 있다.

상위 클래스나 상위 인터페이스에 있는 프로퍼티나 메서드를 오버라이드할 때는 `overrride`변경자를 반드시 사용해야 한다. 인터페이스 메서드는 디폴트 구현을 제공할 수 있다.

```kotlin
interface Focusable {
    fun setFocus(b: Boolean) = println("I ${if (b) "got" else "lost"} focus.")
    fun showOff() = println("I'm focusable!")
}
```

