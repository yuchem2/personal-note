## 타입 캐스팅
[스마트 캐스트](스마트%20캐스트.md)을 하지 못하는 경우에  원하는 타입으로 명시적으로 타입 캐스팅을 하려면 `as` 키워드를 사용하여 타입 캐스팅을 할 수 있다.

```kotlin
val n = e as Num
```
## 임포트
임포트를 할 때 `as` 키워드를 사용하여 임포트한 클래스나 함수를 다른 이름으로 부를 수 있다.

```kotlin
import strings.lastChar as last
import strings.Expression as Expr
```
