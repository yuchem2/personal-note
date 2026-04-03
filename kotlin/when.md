[Kotlin](Kotlin.md)에서 `when`은 조건에 따라 여러 경우를 분기할 때 사용할 수 있는 분기식이다. `if`처럼 식으로 판단되며 값으로 사용될 수 있다.
## 분기 조건: 객체
> [Enum](Enum.md)에서의 Color 클래스 활용
```kotlin
// switch 처럼 사용하기
when (객체) {
    값1 -> 결과1
    값2 -> 결과2
    ...
    조건5, 조건6, 조건7 -> 결과5 // 여러 값을 `,`로 구분하여 하나의 값을 나타낼 수 있다.
    else -> 기본값
}

// 식의 대상 값을 변수에 넣을 수 있다.
when (val color = mesureColor()) {
    RED, ORANGE, YELLOW -> "warm"
    GREEN -> "neutral"
    BLUE, INDIGO, VIOLET -> "cold"
}
```

컴파일러는 `when`이 철저한지 살펴보게 된다. 이는 모든 가능한 경로에서 값을 만들어야 한다. 위의 예시처럼 모든 경우를 처리하지 않는 경우에는 `else` 키워드를 통해 디폴트 케이스를 제공해야 한다.

```kotlin
fun mix(c1: Color, c2: Color) = 
    // setOf는 집합인 Set 객체를 만드는 함수
    when (setOf(c1, c2)) {
        setOf(RED, YELLOW) -> ORANGE
        setOf(YELLOW, BLUE) -> GREEN
        setOf(BLUE, VIOLET) -> INDIGO
        else -> throw Exception("Dirty color")
    }
```

`when`의 분기 조건에는 임의의 객체가 모두 올 수 있다. 코틀린은 **분기 조건에 있는 객체 사이를 비교할때 동등성을 사용한다.** 
## 인자 없는 사용
`when`에 아무 인자가 없으면 각 분기 조건이 `Boolean` 결과를 계산하는 식이어야 한다.

```kotlin
when {
    조건1 -> 결과
    조건2 -> 결과
    ...
    else -> 기본값
}
```
