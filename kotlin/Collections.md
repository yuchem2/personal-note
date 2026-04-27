자료구조를 제공하기 위한 라이브러리.

코틀린에서는 자체 `Collection`을 제공하지 않고, 표준 자바 `Collection` 클래스를 사용하여 `Collection`을 제공하고 있다.

하지만 자바와 달리  `Kotlin Collection Interface`는 기본으로 읽기 전용이다. 또한, 기본적으로 자바 `Collection`과 똑같은 클래스이지만 자바보다 더 많은 기본 기능을 제공하고 있다.
## 컬렉션 처리

### 자바 컬렉션 API 확장
코틀린은 자바 컬렉션에 대한 새로운 기능을 제공하기 위해 [확장 함수](클래스%20확장.md)를 활용하고 있다. 

```kotlin
fun <T> List<T>.last(): T { /* 마지막 원소 반환 */ }
fun Collection<Int>.max(): Int { /* 컬렉션의 최대값 반환 */ }
```

코틀린 표준 라이브러리는 위와 같은 수많은 확장 함수를 포함한다. IDE를 통해 일반 함수와 모든 확장 함수를 확인할 수 있고, [표준 라이브러리 참조 매뉴얼](https://kotlinlang.org/api/latest/jvm/stdlib/)을 통해 각 라이브러리 클래스가 제공하는 모든 메서드를 확인할 수 있다.
### 컬렉션 생성 함수
- `List<T>`를 생성하려면 `listOf`를 사용할 수 있다.
- `Map<T>`를 생성하려면 `mapOf`를 사용할 수 있다.

