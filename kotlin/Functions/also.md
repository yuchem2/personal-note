```kotlin
val fruits = listOf("Apple", "Banana", "Cherry")
val upppercaseFruits = mutableListOf<String>()
val reversedLongFruits = fruits
    .map { it.uppercase() }
    .also { uppercaseFruits.addAll(it) }
    .fitler { it.length > 5 }
    .also { println(it) }
    .reversed()
```
`also`는 [`apply`](apply.md)와 마찬가지로 수신 객체를 받으며, 그 수신 객체에 대한 어떠한 동작(람다)을 수행한 뒤 수신 객체를 돌려준다. 주요한 차이는 람다 안에서 수신 객체를 인자로 참조한다는 것이다. 따라서 파라미터 이름을 부여하거나 디폴트 이름인 `it`을 사용해야 한다.