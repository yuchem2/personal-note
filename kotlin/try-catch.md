## throw
[Kotlin](Kotlin.md)에서 `throw` 키워드를 통해 예외를 던질 수 있다. 자바와 달리 `throw`는 식이므로 다른 식에 포함될 수 있다.

## try, catch, finally
예외를 받는 쪽에서는 `try`와 `catch`, `finally`를 활용해야 한다.

```kotlin
fun readNumber(reader: BufferReder) {
    val result = 
        try {
            Integer.parseInt(reader.readLine())
        } catch(e: NumberFormatException) {
            null
        } finally {
            reader.close()
        }
    println(result)
}

```

자바에서는 위 코드를 함수로 선언할 때 함수 선언에 `throws IOExcpetion`을 붙여 체크 예외(명시적으로 처리해야만 하는 예외)를 표시해야 한다. 그러나 코틀린에서는 체크 예외와 언체크 예외를 구별하지 않는다.

 `try`는 [값을 반환하는 블록](값을%20반환하는%20블록.md)에 명시되어 있는 것처럼 값을 식이기 때문에 값을 코드 블록의 마지막 값을 결과로 사용할 수 있다. 다만 `if`와 달리 반드시 본문을 중괄호로 둘러싸야 한다.
 