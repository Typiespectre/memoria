# Saturday, June 4, 2022

[[Journal]]
[[Javascript]]

## 엄격 모드

[https://ko.javascript.info/strict-mode](https://ko.javascript.info/strict-mode)

엄격 모드(strict mode)를 활성화 했을 때만 ES5의 변경 사항이 실행된다. `"ust strict"`가 스크립트 최상단에 오면 스크립트 전체가 "모던한" 방식으로 동작한다. 혹은 함수 본문 맨 앞에 오면, 해당 함수만 엄격 모드로 실행된다(엄격 모드는 대개 스크립트 전체에 적용하지만 말이다). 또한 엄격 모드가 한 번 실행되면, 이를 적용 해제할 방법은 없다.

하지만 모든 경우에 `"use strict"`를 써놓을 필요는 없다. 모든 자바스크립트의 '클래스'와 '모듈'을 사용하면 엄격 모드가 자동으로 적용되기 때문이다. (해당 튜토리얼에선 엄격 모드가 적용된다.)

## 변수와 상수

[https://ko.javascript.info/variables](https://ko.javascript.info/variables)

### 변수

변수(variable)는 데이터를 저장할 때 쓰이는 '이름이 붙은 저장소'이다. 자바스크립트에선 `let` 키워드를 사용해 변수를 생성한다. 선언(생성)된 변수의 데이터는 메모리에 저장되었기에, 변수명을 이용하면 해당 데이터에 접근할 수 있다.

```js
let message;
message = 'Hello!';     // 문자열을 저장합니다.

// or

let message = 'Hello!'; // 변수를 정의하고 값을 할당합니다.

alert(message); // 변수에 저장된 값을 보여줍니다.
```

* `var` 키워드는 `let`과 거의 동일하게 동작되지만, `var`은 '오래된' 방식이다. 이후에 더 자세하게 다룰 예정이라고 한다.

변수 두 개를 선언하고, 한 변수의 데이터를 다른 변수에 복사할 수도 있다.

```js
let Hello = 'Hello world!';

let message;

// Hello의 'Hello world' 값을 message에 복사합니다.
message = Hello;

// 이제 두 변수는 같은 데이터를 가집니다.
alert(Hello); // Hello world!
alert(message); // Hello world!
```

* 같은 변수를 두 번 선언하면 에러가 발생한다. 선언한 변수를 참조할 때는 `let` 없이 변수명만 사용해야 한다.

* 함수형(functional) 프로그래밍 언어는 변숫값 변경을 금지한다. 즉 이전 변수를 재사용할 수 없다. 변수에 다른 값을 저장하고 싶다면 새 변수를 선언해야 한다.

### 변수 명명 규칙

자바스크립트에선 변수 명명 시 두 가지 제약이 있다:

1. 변수명에는 오직 문자와 숫자, 그리고 기호 `$`와 `_`만 들어갈 수 있다.
2. 변수의 첫 글자는 숫자가 될 수 없다.

또한, 자바스크립트 내부에서 이미 사용하고 있는 예약어(`let, class, return, function ...`)도 변수명으로 사용할 수 없다.

자바스크립트는 카멜 표기법(camelCase)를 흔히 사용한다.

* `use strict` 모드에선 `let` 없이도 변수명을 선언할 수 있지만, 권유하지 않는다.

### 상수

변화하지 않는 변수를 선언할 땐, `const`를 사용한다. 이러한 변수를 '상수(constant)`라고 부른다. 상수는 재할당할 수 없으므로, 변경을 시도하면 에러가 발생한다.

```js
const myBirthday = '18.04.1982';

myBirthday = '01.01.2001'; // error, can't reassign the constant!
```

### 대문자 상수

기억하기 힘든 값을 변수에 할당해 별칭으로 사용하는 관습이 있다. 즉, 대문자 상수는 '하드 코딩한' 값의 별칭을 만들 때 사용하면 편리하다.

```js
const COLOR_RED = "#F00";
const COLOR_GREEN = "#0F0";
const COLOR_BLUE = "#00F";
const COLOR_ORANGE = "#FF7F00";

// 색상을 고르고 싶을 때 별칭을 사용할 수 있게 되었습니다.
let color = COLOR_ORANGE;
alert(color); // #FF7F00
```

### 바람직한 변수명

변수명은 간결하고 명확해야 한다. 변수가 담고있는 것이 무엇인지 잘 설명할 수 있어야 한다. 아래는 변수 명명 시 참고하기 좋은 규칙들이다:

* 사람이 읽을 수 있는 이름을 사용하기(`userName`).
* 단축되거나 짧은 변수명은 피하기.
* 최대한 서술적이고 간결하게 작성하기. `data`와 `value`는 아무것도 설명하지 않기에 나쁜 이름의 예이다.
* 자신만의 규칙이나 소속된 팀의 규칙을 따르기.

변수를 재사용하는 것보단, 변수를 추가하는 것이 훨씬 낫다. 변수를 추가한다고 해서 성능 이슈가 생기지 않는다. 모던 자바스크립트의 압축기(minifier)와 브라우저가 자동으로 코드 최적화를 돕기 때문이다.

## 자료형

[https://ko.javascript.info/types](https://ko.javascript.info/types)

자바스크립트에는 여덟 가지 기본 자료형이 있다. 자바스크립트의 변수는 자료형에 관계없이 모든 데이터를 담을 수 있다. 이처럼 자료의 타입은 있지만 변수에 저장되는 값의 타입은 언제든지 바꿀 수 있는 언어를  **'동적 타입(dynamically typed)'** 언어라고 부른다.

### 숫자형

*숫자형(number type)*은 정수 및 부동소수점 숫자(floating point numver)를 나타낸다. 숫자형과 관련된 연산은 대표적으로 곱셈(`*`), 나눗셈(`/`), 덧셈(`+`) 그리고 뺄셈(`-`)이 있다. 숫자형엔 일반적인 숫자 외에 `Infinity`, `-Infinity`, `NaN`과 같은 '특수 숫자 값(special numver value)'이 포함된다.

* `Infinity`는 어떤 숫자보다 더 큰 특수 값, 무한대를 나타낸다. 어느 숫자든 0으로 나누면 무한대를 얻을 수 있다. `Infinity`를 직접 참조할 수도 있다.

```js
alert ( 1 / 0 );    // 무한대
alert ( Infinity ); // 무한대
```

* `NaN`은 계산 중에 에러가 발생했다는 것을 나타내는 값이다. 부정확하거나 정의되지 않은 수학 연산을 사용하면 계산 중에 에러가 발생하는데, 이때 `NaN`이 반환된다. `NaN`에 어떤 연산을 추가해도 `NaN`이 반환된다.

```js
alert( "숫자가 아님" / 2 ); // NaN, 문자열을 숫자로 나누면 오류가 발생합니다.
alert( "숫자가 아님" / 2 + 5 ); // NaN
```

* 따라서, 자바스크립트에서 행해지는 수학 연산은 '안전'하다고 볼 수 있다. 0으로 나누거나 문자열을 숫자로 취급하는 등의 이례적인 연산이 가능하기 때문이다. 말이 안되는 수학 연산을 하더라도 스크립트는 치명적인 에러를 내뿜으며 죽지 않는다. `NaN`을 반환하며 연산이 종료될 뿐이다.

### BigInt

자바스크립트는 `(2^53-1) (9007199254740991)`보다 큰 값 혹은 `-(2^53-1)`보다 작은 정수는 '숫자형'을 사용해서 나타낼 수 없다. 하지만 암호 관련 작업과 같이 아주 큰 숫자 혹은 정밀도가 필요한 경우, `BigInt`형을 사용할 수 있다.

`BigInt`형은 표준으로 채택된지 얼마 안 된 자료형으로(현재 Firefox, Chrome, Edge, Safari에서 지원하고 있다), 길이에 상관없이 정수를 나타낼 수 있다. `BigInt`형 값은 정수 리터럴 끝에 `n`을 붙인다.

```js
// 끝에 'n'이 붙으면 BigInt형 자료입니다.
const bigInt = 1234567890123456789012345678901234567890n;
```

### 문자형

자바스크립트에선 문자열(string)을 따옴표로 묶는다. 따옴표에는 세 종류가 있다:

1. 큰따옴표: `"Hello"`
2. 작은따옴표: `'Hello'`
3. 역 따옴표(백틱, backtick): ``Hello``

```js
let str = "Hello";
let str2 = 'Single quotes are ok too';
let phrase = `can embed another ${str}`;
```

큰따옴표와 작은따옴표는 '기본적인' 따옴표로, 자바스크립트에서는 이 둘에 차이를 두지 않는다.

역따옴표로 변수나 표현식을 감싼 후 `${...}` 안에 넣어주면, 원하는 변수나 표현식을 문자열 중간에 쉽게 넣을 수 있다.

```js
let name = "John";

// 변수를 문자열 중간에 삽입
alert( `Hello, ${name}!` ); // Hello, John!

// 표현식을 문자열 중간에 삽입
alert( `the result is ${1 + 2}` ); // the result is 3
```

* 자바스크립트는 C언어와 Java처럼 글자형(`char`)을 지원하지 않는다.

### 불린형

불린형(논리 타입)은 `true`와 `false` 두 가지 값만 있는 자료형이다. 불린형은 긍정이나 부정을 나타내는 값을 저장할 때 사용하거나, 비교 결과를 저장할 때 사용한다.

```js
let nameFieldChecked = true; // 네, name field가 확인되었습니다(checked).
let ageFieldChecked = false; // 아니요, age field를 확인하지 않았습니다(not checked)

let isGreater = 4 > 1;

alert( isGreater ); // true (비교 결과: "yes")
```

### `null`값

`null`값은 어느 자료형에도 속하지 않는 값이다. `null`값은 오로지 `null`값만 포함하는 별도의 자료형을 만든다.

자바스크립트의 `null`은 자바스크립트 이외 언어의 `null`과 성격이 다르다. 다른 언어에선 `null`을 '존재하지 않는 객체에 대한 참조'나 '널 포인터(null pointer)'를 나타낼 때 사용한다. 하지만 자바스크립트에선 `null`을 **'존재하지 않는(nothing)'** 값, **'비어 있는(empty)`** 값 그리고 **'알 수 없는(unknown)'** 값을 나타낼 때 사용한다.

```js
let age = null; // age를 알 수 없거나 age의 값이 비어있음
```

### 'undefined' 값

`undefined`값도 `null`값처럼 자신만의 자료형을 형성한다. `undefined`는 **'값이 할당되지 않은 상태'**를 나타낼 때 사용한다. 변수를 선언했지만 값을 할당하지 않았다면 해당 변수에 `undefined`가 자동으로 할당된다.

```js
let age = null;

alert(age); // 'undefined'가 출력됩니다.
```

개밸자가 변수에 `undefined`를 명시적으로 할당하는 것도 가능하긴 하다(`let age = undefined`). 하지만 이러한 방식으로 직접 할당하는 것은 권장하지 않는다. 변수가 *'비어있거나'* *'알 수 없는'* 상태라는 것을 나타내려면 `null`을 사용하고, `undefined`는 값이 할당되지 않은 변수의 초기값을 위해 예약어로 남겨두자.

### 객체와 심볼

일반적인 자료형은 문자열이든 숫자든 한 가지만 표현할 수 있는 원시(primitive) 자료형이다. 반면 `객체(object)` 자료형은 데이터 컬렉션이나 복잡한 개체(entity)를 표현할 수 있다.

`심볼(symbol)` 자료형은 객체의 고유한 식별자(unique identifier)를 만들 때 사용된다. 객체와 심볼은 이후에 다시...

### typeof 연산자

`typeof` 연산자는 이눗의 자료형을 반환한다. 자료형에 따라 처리 방식을 다르게 하고 싶거나 변수의 자료형을 빠르게 알아내고자 할 때 유용하다. `typeof` 연산자는 두 가지 형태의 문법을 지원한다:

1. 연산자: `typeof x`
2. 함수: `typeof(x)`

`typeof x` 혹은 `typeof(x)`를 호출하면 인수의 자료형을 나타내는 문자열이 반환된다.

```js
typeof undefined // "undefined"

typeof 0 // "number"

typeof 10n // "bigint"

typeof true // "boolean"

typeof "foo" // "string"

typeof Symbol("id") // "symbol"

typeof Math // "object"  (1)

typeof null // "object"  (2)

typeof alert // "function"  (3)
```

* (1): `Math`는 수학 연산을 제공하는 내장 객체이므로 `"object"`가 출력된다.
* (2): `null`은 별도의 고유한 자료형을 가지는 특수 값이지, 객체가 아니다(즉, 언어상 오류이다). 하지만 자바스크립트는 하위 호환성을 위해 이런 오류를 수정하지 않고 그대로 남겨두고 있다고...
* (3): `typeof`는 피연산자가 함수면 `"function"`을 반환한다. 그런데 사실 '함수형'은 따로 존재하지 않는다. 함수는 객체형에 속한다. 역시 이는 형식적 오류이지만, 하위 호환성 유지를 위해 남겨진 상태.

## alert, prompt, confirm을 이용한 상호작용

[https://ko.javascript.info/alert-prompt-confirm](https://ko.javascript.info/alert-prompt-confirm)

### alert

```js
alert("Hello");
```

메세지가 있는 모달 창(modal window)를 띄운다. '모달'에는 해당 페이지의 나머지 부분과 상호 작용이 불가능하다는 의미가 담겨있다. 즉, 사용자는 페이지 내의 확인 버튼을 누르기 전까진 창 바깥과 상호작용 할 수 없다.

### prompt

```js
result = prompt(title, [default]);
```

함수가 실행되면 텍스트 메세지와 입력 필드(input field), 확인(OK) 및 취소(Cancel) 버튼이 있는 모달 창을 띄운다.

* `title`: 사용자에게 보여줄 문자열
* `default`: 입력 필드의 초깃값(선택값, 즉 필수로 입력하지 않아도 된다)

`prompt` 함수는 사용자가 입력 필드에 기재한 문자열을 반환한다. 사용자가 입력을 취소한 경우는 `null`이 반환된다.

```js
let age = prompt('나이를 입력해주세요.', 100); // 초깃값으로 100이 설정.

alert(`당신의 나이는 ${age}살 입니다.`); // 당신의 나이는 100살입니다.
```

### confirm 대화상자

```js
result = confirm(question);
```

`confirm` 함수는 매개변수로 받은 `question(질문)`과 확인 및 취소 버튼이 있는 모달 창을 보여준다. 사용자가 확인 버튼을 누르면 `true`를, 그 외에는 `false`를 반환한다.

```js
let isBoss = confirm("당신이 주인인가요?");

alert( isBoss ); // 확인 버튼을 눌렀다면 true가 출력됩니다.
```

위 함수들은 모두 모달 창을 띄우는데, 모달 창이 떠 있는 동안은 스크립트 실행이 일시 중단된다. 사용자가 창을 닫기 전까진 나머지 페이지와 상호작용이 불가능하다.

또한 모달 창에는 두 가지 제약사항이 있다:

1. 모달 창의 위치는 브라우저가 결정하는데, 대개 브라우저 중앙에 위치한다.
2. 모달 창의 모양은 브라우저마다 다르다. 개발자는 모달 창의 모양을 수정할 수 없다.

## 형 변환

[https://ko.javascript.info/type-conversions](https://ko.javascript.info/type-conversions)

함수와 연산자에 전달되는 값은 대부분 적절한 자료형으로 자동 변환되는데(가령, `alert`의 매개변수는 문자형으로 자동 변환됨), 이를 "형 변환(type conversion)"이라 부른다(객체의 형변환은 여기서는 다루지 않는다).

### 문자형으로 변환

`String(value)` 함수를 호출해 전달받은 값을 문자열로 변환할 수 있다.

```js
let value = true;
alert(typeof value); // boolean

value = String(value); // 변수 value엔 문자열 "true"가 저장됩니다.
alert(typeof value); // string
```

### 숫자형으로 변환

숫자형으로의 변환은 수학과 관련된 함수와 표현식에서 자동으로 일어난다(`alert( "6" / "2" )`). `Number(value)` 함수를 이용하면 주어진 값을 숫자형으로 명시해서 변환할 수 있다. 한편, 숫자 이외의 글자가 들어있는 문자열을 숫자형으로 변환하려 하면, 결과는 `NaN`이 된다.

```js
let str = "123";
alert(typeof str); // string

let num = Number(str); // 문자열 "123"이 숫자 123으로 변환됩니다.

alert(typeof num); // number
```

```js
let age = Number("임의의 문자열 123");

alert(age); // NaN, 형 변환이 실패합니다.
```

| 전달받은 값     | 형 변환 후                                                                                                                                               |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `undefined`     | `NaN`                                                                                                                                                    |
| `null`          | `0`                                                                                                                                                      |
| `true`, `false` | `1`, `0`                                                                                                                                                 |
| `string`        | 문자열의 처음과 끝 공백이 제거된다. 공백 제거 후 남아있는 문자열이 없다면 `0`, 그렇지 않다면 문자열에서 숫자를 읽는다. 변환에 실패하면 `NaN`이 반환된다. |

```js
alert( Number("   123   ") ); // 123
alert( Number("123z") );      // NaN ("z"를 숫자로 변환하는 데 실패함)
alert( Number(true) );        // 1
alert( Number(false) );       // 0
```

* `null`과 `undefined`의 숫자형 변환 결과가 다르다는 점에 유의해야 한다. `null`은 `0`이지만 `undefined`는 `NaN`이다.

### 불린형으로 변환

`Boolean(value)`를 호출하면 명시적으로 불리언으로의 형 변환을 수행할 수 있다. 불린형으로 변환 시 적용되는 규칙은 다음과 같다:

* 숫자 `0`, 빈 문자열, `null`, `undefined`, `NaN`과 같이 직관적으로 "비어있다고" 느껴지는 값들은 `false`가 된다.
* 그 이외의 값은 `true`로 변환된다.
* 주의! 문자열 `"0"`과 공백이 있는 문자열은 `true`이다.

```js
alert( Boolean(1) ); // 숫자 1(true)
alert( Boolean(0) ); // 숫자 0(false)

alert( Boolean("hello") ); // 문자열(true)
alert( Boolean("") ); // 빈 문자열(false)

alert( Boolean("0") ); // true
alert( Boolean(" ") ); // 공백이 있는 문자열도 비어있지 않은 문자열이기 때문에 true로 변환됩니다.
```