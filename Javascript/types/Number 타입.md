[배정밀도 64비트 부동소수점 형식 (IEEE 754)](https://ko.wikipedia.org/wiki/%EB%B0%B0%EC%A0%95%EB%B0%80%EB%8F%84_%EB%B6%80%EB%8F%99%EC%86%8C%EC%88%98%EC%A0%90%EC%88%98)의 숫자 데이터 타입으로, 정수나 실수 구분 없이 모든 숫자 값을 관리하는 데이터 타입이다. 
- 안전한 범위: $-(2^{53}-1) \leq x \leq-2^{53}-1$ 
- 저장 가능한 범위
    - 양수: $2^{-1074}$(`Number.MIN_VALUE`) $\leq x \leq 2^{1024}$(`Number.MAX_VALUE`)
    - 음수: $-2^{-1074} \leq x \leq -2^{1024}$
```js
// 모두 같은 number 타입
var integer = 10;
var double = 10.12;
var negative = -20;
```

그러므로 정수, 실수, 2진수, 16진수 리터럴 모두 배정밀도 64비트 부동소수점 형식의 2진수로 저장되며 이를 표현하기 위한 다른 타입을 제공하지 않기 때문에 이들 값을 참조하면 모두 **10진수 실수**로 해석된다.

```js
var binary = 0b01000001;
var octal = 0o101;
var hex = 0x41;

console.log(binary, octal, hex); // 65 65 65
console.log(binary === octal, octal === hex); // true true
```

숫자 타입은 추가적으로 특별한 값을 포함하고 있다.
- `Infinity`: 양의 무한대
- `-Infinity`: 음의 무한대
- `NaN`: 산술 연산 불가(not-a-number)

안전한 범위를 넘는 정수는 JS에서는 안전하게 표현할 수 없어 배정밀도 부동 소수점 근사값으로 표현되며 `Number.isSafeInteger()`를 사용하여 숫자가 안전한 범위에 있는지 확인할 수 있다. 또한 저장 가능한 범위를 넘어서는 값은 다음과 같이 자동으로 변환된다.
- `Number.MIN_VALUE`보다 큰 양수는 `+Infinity`로 변환
- `Number.MAX_VALUE`보다 작은 양수는 `+0`로 변환
- `Number.MIN_VALUE`보다 큰 음수는 `-Infinity`로 변환
- `Number.MAX_VALUE`보다 작은 음수는 `-0`로 변환

또한, `0`은 `-0`과 `+0`으로 표현될 수 있다. 그러나 `+0 === -0`은 `true`로 표현된다. 그러나 `0`으로 나누면 다음과 같이 표현되어 차이가 생긴다.

```js
console.log(42 / +0); // Infinity
console.log(42 / -0); // Infinity
```
