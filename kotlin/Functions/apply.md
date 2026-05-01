```kotlin
fun alphabet() = StringBuilder().apply {
    for (letter in 'A'..'Z') {
        append(letter)
    }
    append("\nNow I know the alphabet!")
}.toString()

fun createViewWithCustomAttributes(context: Context) = 
    TextView(context).apply {
        text = "Sample Text"
        textSize = 20.0
        setPadding(10, 0, 0, 0)
    }
```
`apply`는 [`with`](with.md)과 거의 동일하게 작동한다. 유일한 차이는 `apply`는 항상 자신에 전달된 수신 객체를 반환한다. 또한, `apply`는 임의의 타입의 확장 함수로 호출할 수 있다. 주로 인스턴스를 만들면서 즉시 프로퍼티 중 일부를 초기화해야하는 경우 사용된다.