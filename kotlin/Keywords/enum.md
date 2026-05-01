[Kotlin](Kotlin.md)에서 이넘(enum)은 `enum class`를 사용하지만, 자바에서는 `enum`을 사용한다.

>코틀린에서 `enum`은 소프트 키워드이다. `class` 앞에 있을 때만 특별한 의미를 지니지만 다른 곳에서 일반적인 이름으로 사용할 수 있다.

```kotlin
enum class Color(
    var r: Int,
    var g: Int,
    var b: Int
) {
    RED(255, 0, 0),
    ORANGE(255, 165, 0),
    YELLOW(255, 255, 0),
    GREEN(0, 255, 0),
    BLUE(0, 0, 255),
    INDIGO(75, 0, 13),
    VIOLET(238, 130, 238); // 이넘 상수 목록
    
    val rgb = (r * 256 + g) * 256 + b // 프로퍼티
    fun printColor() = println("$this is $rgb") // 메서드
}
```

- 이넘 클래스 안에 메서드를 정의하는 경우 이넘 상수 목록과 메서드 정의 사이에 세미클론을 반드시 추가해야 한다.
- `import`할 때 상수 값들을 `import colors.Color.*`와 같이 사용하여 임포트할 수 있다.
