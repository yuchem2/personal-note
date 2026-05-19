## 타입 캐스팅
[스마트 캐스트](스마트%20캐스트.md)을 하지 못하는 경우에  원하는 타입으로 명시적으로 타입 캐스팅을 하려면 `as` 키워드를 사용하여 타입 캐스팅을 할 수 있다.

```kotlin
val n = e as Num
```
## 안전한 타입 캐스팅
`?`를 함께 사용하면 안전한 타입 캐스팅을 할 수 있다. `as?` 연산자는 어떤 값을 지정한 타입으로 변환한다. 이때 값을 대상 타입으로 변환할 수 없으면 `null`을 반환한다. 이 연산자를 사용하는 일반적인 패턴은 캐스트를 수행한 뒤에 [엘비스 연산자](엘비스%20연산자.md)를 사용하는 것이다.

```kotlin
class Person(val firstName: String, val lastName: String) {
    override fun equals(other: Any?): Boolean {
        val otherPerson = other as? Person ?: return false
        return otherPerson.firstName == firstName && otherPerson.lastName == lastName
    }
}
```

## 임포트
임포트를 할 때 `as` 키워드를 사용하여 임포트한 클래스나 함수를 다른 이름으로 부를 수 있다.

```kotlin
import strings.lastChar as last
import strings.Expression as Expr
```
