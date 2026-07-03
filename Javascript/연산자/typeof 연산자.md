피연산자의 [데이터 타입](데이터%20타입.md)을 [String 타입](String%20타입.md)의 값으로 반환하는 연산자로, 항상 `"string"`, `"number"`, `"bigint"`, `"boolean"`, `"undefined"`, `"object"`, `"function"` 중 하나를 반환한다. 

> `typeof` 연산자로 `null`을 연산(`null typeof`)하면 `"null"`이 아닌 `"object"`를 반환하여 주의가 필요하다. 이는 JS의 첫 번째 버전의 버그이다.

`typeof` 연산자로 선언하지 않은 식별자를 연산하면 `ReferenceError`가 아닌 `undefined`를 반환하기 때문에 주의할 필요가 있다.
