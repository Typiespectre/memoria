# Friday, June 17, 2022

[[Journal]]
[[javascript]]

## 원시값의 메서드

[https://ko.javascript.info/primitives-methods](https://ko.javascript.info/primitives-methods)

원시값은 `문자(string)`, `숫자(number)`, `bigint`, `불린(boolean)`, `심볼(symbol)`, `null`, `undefined` 형으로 총 일곱 가지이다.

객체는 프로퍼티에 다양한 종류의 값을 저장할 수 있다. 대괄호 `{}`를 사용해 만들 수 있다. 자바스크립트에는 여러 종류의 객체가 있으며, 함수도 객체의 일종이다.

객체의 장점 중 하나는 함수를 프로퍼티로 저장할 수 있다는 것이다.

```js
let john = {
  name: "John",
  sayHi: function() {
    alert("친구야 반갑다!");    // 메서드
  }
};

john.sayHi(); // 친구야 반갑다!
```

자바스크립트는 다양한 객체를 가지고 있지만, 그런 기능을 사용하면 시스템 자원이 많이 소모되는 단점이 있다.

### 원시값을 객체처럼 사용하기

'원시 래퍼 객체(object wrapper)'는 원시값을 가볍게 유지하면서 메서드를 호출할 수 있도록 돕는다.

```js
let str = "Hello";

alert( str.toUpperCase() ); // HELLO
```

1. 문자열 `str`은 원시값이므로 원시값의 프로퍼티(`toUpperCase`)에 접근하는 순간 특별한 객체가 만들어진다.
2. 메서드가 실행되고 새로운 문자열이 반환된다.
3. 특별한 객체는 파괴되고, 원시값 `str`만 남는다.

* `null/undefined`는 메서드를 가지지 않는다!

## 숫자형

[https://ko.javascript.info/number](https://ko.javascript.info/number)

자바스크립트 숫자형은 과학적 표기법을 사용한다(`1e3`: 1 * 1000, `1e-3`: 1 / 1000).
16진수는 `0xff`와 같은 방식으로 표시한다. 2진수는 `0b1111`, 8진수는 `0o333`과 같이 사용. 이외의 진법은 `parseInt`를 사용해야 한다.

`num.toString(base)` 메서드는 `base` 진법으로 `num`을 표현한 후, 이를 문자형으로 반환해 변환한다.

```js
let num = 255;

alert( num.toString(16) );  // ff
alert( num.toString(2) );   // 11111111
```

어림수 관련 함수는 다음과 같다:

* `Math.floor` : 소수점 첫째 자리에서 버림
* `Math.ceil` : 소수점 첫째 자리에서 올림
* `Math.round` : 소수점 첫째 자리에서 반올림
* `Math.trunc` : 소수부를 무시함.

소수 정밀도 이슈를 해결하기 위해선 `toFixed`를 이용하는 방법이 있다.

```js
let sum = 0.1 + 0.2;
alert( +sum.toFixed(2) ); // 0.3
```

`NaN`은 자기 자신을 포함하여 그 어떤 값과도 같지 않다는 독특한 속성이 있다. 그렇기에 어떤 값이 `NaN`임을 확인하는 방법으로, `isNaN(value)`라는 함수를 사용할 수 있다.

```js
alert( NaN === NaN ); // false

alert( isNaN(NaN) ); // true
alert( isNaN("str") ); // true
```

`isFinite(value)` 함수로 어떤 숫자가 무한(`Infinity/-Infinity`)이거나 `NaN`인지를 검사할 수 있다.

`parseInt`와 `parseFloat`는 문자에 도달하기 전까지 숫자를 읽는다. 마치 아토이 함수처럼...

```js
alert( parseInt('100px') ); // 100
alert( parseFloat('12.5em') ); // 12.5

alert( parseInt('12.3') ); // 12, 정수 부분만 반환됩니다.
alert( parseFloat('12.3.4') ); // 12.3, 두 번째 점에서 숫자 읽기를 멈춥니다.

alert( parseInt('a123') ); // NaN, a는 숫자가 아니므로 숫자를 읽는 게 중지됩니다.
```

`parseInt`에 두 번째 매개변수를 사용하면 진수로 변환할 수 있다.

```js
alert( parseInt('0xff', 16) ); // 255
alert( parseInt('ff', 16) ); // 255, 0x가 없어도 동작합니다.

alert( parseInt('2n9c', 36) ); // 123456
```

## 문자열

[https://ko.javascript.info/string](https://ko.javascript.info/string)

자바스크립트의 텍스트는 모두 문자열 형태로 저장된다.

백틱으로 표현식을 문자열 중간에 삽입하는 방식을 템플릿 리터럴(template literal)이라고 부른다.

```js
function sum(a, b) {
  return a + b;
}

alert(`1 + 2 = ${sum(1, 2)}.`); // 1 + 2 = 3.
```

작은따옴표나 큰따옴표를 사용하면 여러 줄짜리 문자열을 만들 수 없다. 하지만 백틱을 사용하면 가능하다!

```js
let guestList = `손님:
 * John
 * Pete
 * Mary
`;

alert(guestList); // 손님 리스트를 여러 줄에 걸쳐 작성함
```

줄바꿈 특수기호 `\n`을 작은따옴표나 큰따옴표 문자열에 넣어서 사용할 수 있다. 역슬래시를 사용하는 방식은 터미널에서와 동일하다.

문자열의 길이를 구하고자 한다면 `length` **프로퍼티**를 사용하면 된다.

```js
alert( `My\n`.length ); // 3
```

파이썬처럼 문자열을 대괄호를 사용해서 접근할 수 있다.

```js
let str = `Hello`;

// 첫 번째 글자
alert( str[0] ); // H
alert( str.charAt(0) ); // H, 하위 호환성을 위해 남아있는 메서드.

// 마지막 글자
alert( str[str.length - 1] ); // o
```

문자열은 수정할 수 없다.

```js
let str = 'Hi';

str[0] = 'h'; // Error: Cannot assign to read only property '0' of string 'Hi'
alert( str[0] ); // 동작하지 않습니다.
```

`str.indexOf(substr, pos)` 메서드를 이용하면, `substr`이 어디에 위치하는지 찾아준다. 원하는 부분 문자열을 찾으면 위치를 반환하고, 그렇지 않으면 `-1`을 한환한다.

```js
let str = 'Widget with id';

alert( str.indexOf('Widget') ); // 0, str은 'Widget'으로 시작함
alert( str.indexOf('widget') ); // -1, indexOf는 대·소문자를 따지므로 원하는 문자열을 찾지 못함

alert( str.indexOf("id") ); // 1, "id"는 첫 번째 위치에서 발견됨 (Widget에서 id)
```

`pos`를 이용하여 검색 위치를 조정할 수 있다.

```js
let str = 'Widget with id';

alert( str.indexOf('id', 2) ) // 12
```

```js
let str = 'As sly as a fox, as strong as an ox';

let target = 'as'; // as를 찾아봅시다.

let pos = 0;
while (true) {
  let foundPos = str.indexOf(target, pos);
  if (foundPos == -1) break;

  alert( `위치: ${foundPos}` );
  pos = foundPos + 1; // 다음 위치를 기준으로 검색을 이어갑니다.
}
```

부분 문자열 포함 여부만 알고자 할 때는, `str.includes(substr, pos)`를 이용하면 좋다. (`true`, `false` 반환)

```js
alert( "Widget".includes("id") ); // true
alert( "Widget".includes("id", 3) ); // false, 세 번째 위치 이후엔 "id"가 없습니다.
```

부분 문자열 추출에는 `str.slice(start [, end])`를 사용하자. 문자열의 `start`부터 `end` 바로 전까지 반환한다. 음수 인덱싱도 허용한다.

```js
let str = "stringify";
alert( str.slice(0, 5) ); // 'strin', 0번째부터 5번째 위치까지(5번째 위치의 글자는 포함하지 않음)
alert( str.slice(0, 1) ); // 's', 0번째부터 1번째 위치까지(1번째 위치의 자는 포함하지 않음)

alert( str.slice(2) ); // ringify, 2번째부터 끝까지

// 끝에서 4번째부터 시작해 끝에서 1번째 위치까지
alert( str.slice(-4, -1) ); // gif
```

문자열 비교와 관련된 사항은 본문을 확인!