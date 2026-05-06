#### 원소 제거와 변환
```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6, 7)
println(numbers.filter { it > 3 })
val filtered = numbers.filterIndexed { index, element -> 
    index % 2 == 0 && element > 3
}

val (comeIn, comeOut) = people.partition { p: Person -> p.age <= 27 }
```
- `filter`: 컬렉션을 순회하면서 주어진 람다가 `true`를 반환하는 원소들 컬렉션 반환
- `filterNot`: 컬렉션을 순회하면서 주어진 람다가 `false`를 반환하는 원소들 컬렉션 반환
- `filterIndexed`: `filter`의 인덱스 버전
- `filterKeys`: 맵 Key 전용 `filter`
- `filterValues`: 맵 Values 전용 `filter`
- `map`: 컬렉션을 순회하면서 람다를 각 원소에 적용하고, 그 결과값 컬렉션 반환
- `mapIndexed`: `map`의 인덱스 버전
- `mapKeys`: 맵 Key 전용 `map`
- `mapValues`: 맵 Values 전용 `map`
- `partition`: 컬렉션을 순회하면서 주어진 람다가 `true`인 원소들, `false`인 원소들의 쌍을 반환
    - 한 컬렉션에 대해서 `filter`과 `filterNot`을 동일한 람다에 적용한 결과와 동일
- `flatten`: 컬렉션의 컬렉션을 평평한 컬렉션으로 반환
- `flapMap`: 컬렉션의 각 원소를 파라미터로 주어진 함수를 사용해 변환한다. 그 후 변환된 결과를 하나의 리스트로 합친 결과를 반환
#### 컬렉션 값 누적
```kotlin
val list = listOf(1, 2, 3, 4)
val summed = list.reduce { acc, element -> acc + element }
val summedList = list.runningReduce { acc, element -> acc + element }
val people = listOf(People("Alex", 29), People("Natalia", 28))
val fold = people.fold("") { acc, person -> acc + person.name }
```
> 빈 컬렉션에 대해 `reduce`는 `RuntimeistException` 발생. `fold`는 초기값을 지정하여 별도의 오류 없음
- `reduce`: 컬렉션의 첫 원소를 누적기에 넣은 후 차례대로 누적하여 리턴
- `ruuningReduce`: `reduce`와 동일하나 중간 단계의 모든 누적 값을 뽑아낼 수 있음
- `fold`: `reduce`와 로직은 동일하나 시작값을 임의로 변경할 수 있다.
- `runningFold`: `fold`와 동일하나 중간 단계의 모든 누적 값을 뽑아낼 수 있음
#### 컬렉션에 술어 적용
- `all`: 모든 원소가 주어진 람다에 의해 `true`가 나오는지 여부 반환
    - 빈 컬렉션에 대해 항상 항상 `true`를 반환
- `any`: 적어도 하나 이상의 원소가 주어진 람다에 의해 `true`가 나오는지 여부 반환.
    - 빈 컬렉션에 대해서 항상 `false`를 반환
    - 주어진 조건을 만족하는 원소를 하나라도 찾으면 즉시 종료.
    - `!all`과 동일
- `none`: 모든 원소가 원소가 주어진 람다에 의해 `false`가 나오는지 여부 반환
    - 빈 컬렉션에 대해 항상 `true`를 반환
    - `!any`와 동일
- `count`: 주어진 람다가 `true`를 반환하는 원소의 개수 반환
- `find`: 주어진 람다가 `true`를 반환하는 첫 원소 반환. 만약 없다면 `null`을 반환
    - 조건을 만족하는 원소를 하나라도 찾으면 즉시 종료.
#### 컬렉션을 맵으로 반환
```kotlin
val groups = people.groupBy { it.age } // Map<Int, List<Person>>
val nameToAge = people.assoicate { it.name to it.age } // Map<String, Int>
val personToAge = people.associateWith { it.age } // Map<Person, Int>
val ageToPerson = people.associateBy { it.age } // Map<Int, Person>
```
> `groupBy`를 제외한 경우 키는 중복을 허용하지 않기 때문에 원소 순서에 의해 마지막 결과가 이전 결과를 덮어쓴다.
- `groupBy`: 컬렉션을 순회하면서 주어진 람다의 결과를 키로 하고, 같은 키를 가지는 원소들의 컬렉션을 값으로 하는 맵 반환
- `associate`: 키/값을 쌍으로 반환하는 람다를 전달하면 각 원소에 해당 람다를 적용하여 맵 반환
- `associateWith`: 컬렉션의 원래 원소를 키로 사용하고, 람다의 결과를 값으로 사용하여 맵 반환
- `associateBy`: 컬렉션의 원래 원소를 값으로 사용하고, 람다의 결과를 키로 사용하여 맵 반호나
#### 가변 컬렉션의 원소 변경
- `replaceAll`: 지정한 람다로 얻은 결과로 모든 원소를 변환한다.
- `fill`: 모든 원소를 같은 값으로 채울 때 사용하며, 람다의 결과나 값으로 모든 원소를 변환
#### 특별한 경우 처리
```kotlin
val empty = emptyList<String>()
val blankName = " "
println(empty.ifEmpty { listOf("no", "values", "here") }) // [no, values, here]
println(blankName.ifEmpty { "(unnamed)" }) // 
println(blanName.ifBlank { "(unnamed)" }) // (unnamed)
```
- `ifEmpty`: 컬렉션에 아무 원소도 없을 때 기본값을 생성하는 람다를 반환
- `ifBlank`: 문자열의 `ifEmpty`로, 공백으로만 이루어진 경우 기본값을 생성하는 람다를 반환
####  컬렉션 나누기
```kotlin
val temperatures = listOf(27.7, 29.8, 22.0, 35.5, 19.1)
println(temperatures.windowed(3))
// [[27.7, 29.8, 22.0], [29.8, 22.0, 35.5], [22.0, 35.5, 19.1]]
println(temperatures.windowed(3) { it.sum() / it.size })
// [26.5, 29.09999999, 25.5333333333]
println(temperatures.chunked(2))
// [[27.7, 29.8], [22.0, 35.5], [19.1]]
pritln(temperatures.chunked(2) { it.sum() })
// [57.5, 57.5, 19.1]
```
- `windowed`: 슬라이딩 윈도우를 생성. 윈도우 크기를 기본적으로 받고, 람다를 받으면 각 윈도우(컬렉션)에 대해 람다를 적용하여 결과 반환
- `chunked`: 서로 겹치지 않는 부분으로 컬렉션을 반환
#### 컬렉션 합치기
```kotlin
val names = listOf("Joe", "Mary", "Jamie")
val ages = listOf(22, 31, 31, 44, 0)
println(names.zip(ages)) // [(Joe, 22), (Mary, 31), (Jamie, 31)]
println(names.zip(ages) { name, age -> Person(name, age) })
// [People(name=Joe, age=22), People(name=Mary, age=31), People(name=Jamie, age=31)]
```
- `zip`: 두 컬렉션에 대해서 같은 인덱스에 있는 원소들의 쌍으로 이뤄진 리스트 반환
    - 결과 컬렉션의 길이는 두 입력 컬렉션 중 더 짧은 쪽의 길이와 같다.
    - 중위 표기법으로도 호출할 수 있으나 이때는 람다를 전달할 수 없다.