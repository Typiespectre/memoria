# Sunday, June 5, 2022

[[Journal]]
[[javascript]]

모든 내용을 정리하는 일은 무리수이니... 기억할 만한 내용만 정리하자.

## 기본 연산자와 수학

[https://ko.javascript.info/operators](https://ko.javascript.info/operators)

이항 덧셈 연산자(`+`)는 문자열 연결과 변환의 기능을 동시에 수행한다. 다른 숫자형 연산자는 문자열을 숫자로 변환하기만 한다.

```js
alert(2 + 2 + '1' ); // '221'이 아니라 '41'이 출력됩니다.

alert( 6 - '2' ); // 4, '2'를 숫자로 바꾼 후 연산이 진행됩니다.
alert( '6' / '2' ); // 3, 두 피연산자가 숫자로 바뀐 후 연산이 진행됩니다.
```

단항 덧셈 연산자는 숫자형 변환의 기능을 한다.

```js
// 숫자형이 아닌 피연산자는 숫자형으로 변화합니다.
alert( +true ); // 1
alert( +"" );   // 0
```

자바스크립트도 복합 할당 연산자(`+=` ...), 증감 연산자(`++`, `--`)를 사용할 수 있다.

쉼표 연산자는 여러 표현식을 코드 한 줄에서 평가할 수 있게 한다. 하지만 값을 반환할 경우, 마지막 표현식의 평가 결과만 반환된다.

```js
let a = (1 + 2, 3 + 4);

alert( a ); // 7 (3 + 4의 결과)

// 사용 예시
for (a = 1, b = 3, c = a * b; a < 10; a++) {
 ...
}
```

## 비교 연산자

[https://ko.javascript.info/comparison](https://ko.javascript.info/comparison)

자바스크립트는 유니코드 순으로 문자열의 크기를 비교한다(또한 길이가 더 긴 것이 더 큰 값을 가진다).

```js
alert( 'Z' > 'A' ); // true
alert( 'Glow' > 'Glee' ); // true
alert( 'Bee' > 'Be' ); // true
```

비교형의 값들은 모두 숫자형으로 변환된다.

```js
alert( '2' > 1 ); // true, 문자열 '2'가 숫자 2로 변환된 후 비교가 진행됩니다.
alert( '01' == 1 ); // true, 문자열 '01'이 숫자 1로 변환된 후 비교가 진행됩니다.
```

**일치 연산자(strict equality operator)**는 자료형의 동등 여부까지 검사한다.

```js
alert( 0 == false ); // true
alert( 0 === false ); // false, 피연산자의 형이 다르기 때문입니다.
```

`null`과 `undefined`의 연산자 관계:

```js
alert( null === undefined ); // false, 두 값의 자료형이 다르기 때문.
alert( null == undefined ); // true, 특별한 규칙에 의해 동등 관계
```

`null`과 `undefined`는 `==`에서 형변환을 하지 않는다. `undefined`와 `null`을 비교하는 경우에만 `true`를 반환하고, 그 이외의 경우에는 거짓을 반환한다. `null`은 오직 `undefined`와만 같다(`==`).

```js
alert( null > 0 );  // false
alert( null == 0 ); // false
alert( null >= 0 ); // true
```

`undefined`는 `NaN`으로 반환되기에, 비교 연산의 경우 항상 `false`를 반환한다(위의 `null`과의 비교를 제외하고)

```js
alert( undefined > 0 ); // false
alert( undefined < 0 ); // false
alert( undefined == 0 ); // false
```

따라서, 일치 연산자(`===`)를 제외한 비교 연산자의 피연산자에 `undefined`나 `null`이 오지 않도록 주의해야 한다.

## if와 '?'를 사용한 조건 처리

[https://ko.javascript.info/ifelse](https://ko.javascript.info/ifelse)

형식은 C언어와 같다.

```js
let year = prompt('ECMAScript-2015 명세는 몇 년도에 출판되었을까요?', '');

if (year < 2015) {
  alert( '숫자를 좀 더 올려보세요.' );
} else if (year > 2015) {
  alert( '숫자를 좀 더 내려보세요.' );
} else {
  alert( '정답입니다!' );
}
```

숫자 `0`, 빈 문자열 `""`, `null`, `undefined`, `NaN`은 모두 불린형 변환 시 `false`가 된다.

조건부(conditional) 연산자, 삼항(ternary) 연산자를 이용하면 조건문을 더 짧고 간결하게 작성할 수 있다.

```js
let accessAllowed;
let age = prompt('나이를 입력해 주세요.', '');

if (age > 18) {
  accessAllowed = true;
} else {
  accessAllowed = false;
}

// 위 조건문은 아래처럼 변환할 수 있다.

let accessAllowed = (age > 18) ? true : false;
```

```js
let age = prompt('나이를 입력해주세요.', 18);

if (age < 3) {
  message = '아기야 안녕?';
} else if (age < 18) {
  message = '안녕!';
} else if (age < 100) {
  message = '환영합니다!';
} else {
  message = '나이가 아주 많으시거나, 나이가 아닌 값을 입력 하셨군요!';
}

// 위 조건문은 아래처럼 변환할 수 있다.

let message = (age < 3) ? '아기야 안녕?' :
  (age < 18) ? '안녕!' :
  (age < 100) ? '환영합니다!' :
  '나이가 아주 많으시거나, 나이가 아닌 값을 입력 하셨군요!';
```

하지만 삼항 연산자는 가독성이 좋지 않기에, 상황에 맞게 사용하는 것이 좋다.

## 논리 연산자

[https://ko.javascript.info/logical-operators](https://ko.javascript.info/logical-operators)

OR(`||`) 연산자를 여러 개 체이닝하면, 첫 번째 참의 값이 반환한다. 만약 피연산자에 참이 하나도 없으면, 마지막 피연산자를 **형 변환 없이** 반환한다.

```js
let firstName = "";
let lastName = "";
let nickName = "바이올렛";

alert( firstName || lastName || nickName || "익명"); // 바이올렛
```

위 코드에서, 모든 변수가 거짓이면, "익명"이 출력된다. 또한, 왼쪽에서 오른쪽으로 값을 평가하다 참을 만나면, 이후의 코드는 실행하지 않는다(그렇기에 모든 값이 거짓이면 맨 마지막 값이 나온다).

```js
true || alert("not printed");
false || alert("printed");
```

AND(`&&`) 연산자는 반대로, 첫 번째 거짓 값을 반환한다. 모든 피연산자가 참이면, 마지막 피연산자를 형 변환 없이 반환한다.

```js
alert( 1 && 2 && null && 3 ); // null

alert( 1 && 2 && 3 ); // 마지막 값, 3
```

참고로, AND 연산자의 우선순위가 OR 연산자보다 높다.

NOT`!` 연산자는 피연산자를 불린형으로 변환하고, 그 값의 역을 반환한다. 따라서 NOT 연산자를 두 개 연달아 사용하면, 피연산자의 값을 불린으로 변환하는 결과가 나오게 된다.

```js
alert( !!"non-empty string" ); // true
alert( !!null ); // false
```

참고! `alert` 메서드는 값을 반환하지 않는다(`undefined`). 따라서, 아래의 코드는 1과 2를 브라우저에 출력한다.

```js
alert( alert(1) || 2 || alert(3) );
```

## nullish 병합 연산자 '??'

nullish 병합 연산자(nullish coalescing operator) `??`를 사용하면 짧은 문법으로 여러 피연산자 중 값이 '확정되어있는' 변수를 찾을 수 있다.

```js
x = a ?? b

// 위 코드는 아래와 같다.

x = (a !== null && a !== undefined) ? a : b;
```

OR 연산자와 nullish 연산자의 차이는:

* `||`는 첫 번째 참의 값을 반환한다.
* `??`는 첫 번째 정의된(defined) 값을 반환한다.

```js
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

따라서, 0이 할당될 수 있는 변수를 사용할 땐, `??`를 사용하는 것이 적합할 수도 있다. `??`는 우선순위가 꽤 낮기에, 괄호를 같이 사용하면 좋다. 또한, 안정성과 관련하여, `??`는 AND 또는 OR 연산자와 함께 사용할 수 없다.