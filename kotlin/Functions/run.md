```kotlin
fun main() {
    run { println(42) }
    val result = StringBuilder().run { 
        append("Hello")
        toString()
    }
}
```

`run`은 하나의 인자만을 가지며 인자로 받은 람다를 실행한다. 또한, 수신 객체 지정 람다로 활용할 수 있다. 임의의 타입의 확장 함수로 호출할 수 있다.