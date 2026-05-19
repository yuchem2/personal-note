```kotlin
fun sendEmailTo(email: String) { /* ... */ }

fun main() {
    val email: String? = "foo@bar.com"
    email?.let { email -> sendEmailTo(email) }
}
```

`let`은 자신의 수신 객체를 인자로 전달받은 람다에게 넘긴다. 가장 흔한 용례는 [널이 될 수 있는 타입](널이%20될%20수%20있는%20타입.md)의 값을 널이 아닌 값만 인자로 받는 함수에 넘기는 경우이다. 
