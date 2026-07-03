`undefined`라는 오직 하나의 값만 가질 수 있는 타입이다. 개념적으로 `undefined`는 값이 없음을 의미한다. 일반적으로 값이 없는 경우 언어의 기본값은 `undefined`이다. **이는 [`var`](var.md)로 생성한 변수가 `undefined`로 초기화되는 이유이다.**

또한, `undefined`는 전역 속성인 일반적인 식별자이며 일반적으로 값이 없는 경우 언어의 기본값은 `undefined`이다.
- 반환 값이 없는 `return` 문은 암시적으로 `undefined`를 반환한다.
- 존재하지 않는 객체 속성에 접근하면 `undefined`가 반환된다.
- 초기화가 없는 변수 선언은 변수를 `undefined`로 암시적으로 초기화한다.
- 대부분의 메서드는 요소를 찾을 수 없을 때 `undefined`를 반환한다.
