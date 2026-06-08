```kotlin
fun sendEmailTo(email: String) { /* ... */ }

fun main() {
    val email: String? = "foo@bar.com"
    // 안전한 호출로 인해 null 검사 이후 람다에서는 널이 될 수 없는 타입으로 간주
    email?.let { email -> sendEmailTo(email) }
    
    val recipient: String? = null
    // 안전한 호출을 하지 않는다. 따라서 it은 널이 될 수 있는 타입으로 간주
    recipient.let { sendEmailTo(it) } 
}
```

`let`은 자신의 수신 객체를 인자로 전달받은 람다에게 넘긴다. 가장 흔한 용례는 [널이 될 수 있는 타입](널이%20될%20수%20있는%20타입.md)의 값을 널이 아닌 값만 인자로 받는 함수에 넘기는 경우이다. **이는 [안전한 호출 연산자](안전한%20호출%20연산자.md)를 이용해 `let`을 사용하면 자동으로 `null` 검사 이후 람다 인자를 널이 될 수 없는 타입으로 추론하기 때문이다.** 하지만 호출할 때 [안전한 호출 연산자](안전한%20호출%20연산자.md)를 사용하지 않고, 호출하면 람다의 인자는 [널이 될 수 있는 타입](널이%20될%20수%20있는%20타입.md)으로 추론된다.
