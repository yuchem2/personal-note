ES11에서 추가된 타입으로 [Number 타입](Number%20타입.md)에서 안전하게 표현할 수 있는 정수의 최대치보다 큰 값을 표현하기 위한 새로운 원시 값이다. 임의 정밀도로 정수를 나타낼 수 있다. BigInt는 정수 [리터럴](리터럴.md) 뒤에 `n`을 넣어 `10n`을 붙이거나 `BigInt` 함수를 호출하여 생성할 수 있다.

```js
const x = BigInt(Number.MAX_SAFE_INTEGER);
console.log(x + 1n === x + 2n); // false
console.log(Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2); // true
```

`+`, `*`, `-`, `**`, `%` 연산자를 사용할 수 있다. 금지된 연산자는 `>>>`뿐이며 [Number 타입](Number%20타입.md)과 엄격하게 같지는 않지만 느슨하게 유사하다.

BigInt는 소수를 나타낼 수는 없지만, 큰 정수를 더 정확하게 나타낼 수 있어 BigInt 값은 숫자보다 항상 더 정확하거나 덜 정확하지 않는다. **그러나 [Number 타입](Number%20타입.md)과 BigInt 타입 간의 연산은 허용되지 않는다.**
