# Tuesday, June 7, 2022

[[Journal]]
[[javascript]]

## while과 for 반복문

[https://ko.javascript.info/while-for](https://ko.javascript.info/while-for)

`while`, `for`문 형식은 C언어와 동일.

삼항 연산자에는 `break`나 `continue`를 사용할 수 없다.

이중 반복문을 한 번에 빠져나오려면, 반복문 앞에 *레이블(label)*을 사용하면 된다.

```js
outer: for (let i = 0; i < 3; i++) {

  for (let j = 0; j < 3; j++) {

    let input = prompt(`(${i},${j})의 값`, '');

    // 사용자가 아무것도 입력하지 않거나 Cancel 버튼을 누르면 두 반복문 모두를 빠져나옵니다.
    if (!input) break outer; // (*)

    // 입력받은 값을 가지고 무언가를 함
  }
}
alert('완료!');
```

하지만, 레이블이 '점프'의 기능을 하는 것은 아니다.

## switch문

[https://ko.javascript.info/switch](https://ko.javascript.info/switch)

C언어와 사용법은 동일함.

## 함수

자바스크립트가 함수를 선언하는 방식은 아래와 같다.

```js
function name(parameters) {
  ...함수 본문...
}
```

지역 변수와 전역 변수 모두 C언어의 문법과 동일하다.

함수 내부에 전역 변수와 동일한 이름을 가진 변수가 선언되면, 외부 변수는 함수 내부에서 영향을 받지 않는다.

매개변수로 들어온 값은 함수 내부에서만 사용된다. 즉 외부 변수에 영향을 주지 않는다.

```js
function showMessage(from, text) {

  from = '*' + from + '*'; // "from"을 좀 더 멋지게 꾸며줍니다.

  alert( from + ': ' + text );
}

let from = "Ann";

showMessage(from, "Hello"); // *Ann*: Hello

// 함수는 복사된 값을 사용하기 때문에 바깥의 "from"은 값이 변경되지 않습니다.
alert( from ); // Ann
```

매개변수에 값을 전달하지 않으면, 오류는 발생하지 않지만 해당 매개변수가 `undefined`가 된다. 매개변수에 값이 전달되지 않더라도 `undefined`가 되지 않게 하려면, '기본값(default value)'를 설정하면 된다.

```js
function showMessage(from, text = "no text given") {
  alert( from + ": " + text );
}

showMessage("Ann"); // Ann: no text given

// 혹은 좀 더 응용하면...

function showMessage(from, text = anotherFunction()) {
  // anotherFunction()은 text값이 없을 때만 호출됨
  // anotherFunction()의 반환 값이 text의 값이 됨
}
```

### 매개변수 기본값을 설정할 수 있는 또 다른 방법

```js
function showMessage(text) {
  if (text === undefined) {
    text = '빈 문자열';
  }

  alert(text);
}

showMessage(); // 빈 문자열

// or

// 매개변수가 생략되었거나 빈 문자열("")이 넘어오면 변수에 '빈 문자열'이 할당됩니다.
function showMessage(text) {
  text = text || '빈 문자열';
  ...
}

// or

// 매개변수 'count'가 넘어오지 않으면 'unknown'을 출력해주는 함수
function showCount(count) {
  alert(count ?? "unknown");
}

showCount(0); // 0
showCount(null); // unknown
showCount(); // unknown
```

자바스크립트의 함수가 리턴값을 반환하는 방식은 C언어와 동일함.

`return`문이 없거나 `return` 지시자만 있는 경우, 함수는 `undefined`를 반환한다.

참고로 `return`문에 절대 새 줄을 삽입하면 안된다!

### 함수 이름짓기

함수는 어떤 동작을 수행하기 위한 코드를 모아놓은 것이기에, 함수의 이름은 대개 동사이다. 함수의 이름은 가능한 간결하고 명확해야 하며, 어떤 동작을 하는지 나타내야 한다.

* `"show"` : 무언가를 보여줌
* `"get"` : 값을 반환함
* `"calc"` : 무언가를 계산함
* `"create"` : 무언가를 생성함
* `"check"` : 무언가를 확인하고 불린값을 반환함

또한 함수는 동작 하나만 담당하는 것이 좋다. 하나의 함수에서 어떤 기능을 수행하고 출력하는 기능까지 모두 담당하는 것은 좋지 않다.

좋은 코드의 예시(즉, 이름만 보고도 어떤 동작을 하는지 알 수 있는 코드를 *자기 설명적(self-describing)* 코드라고 한다).

```js
function showPrimes(n) {

  for (let i = 2; i < n; i++) {
    if (!isPrime(i)) continue;

    alert(i);  // a prime
  }
}

function isPrime(n) {
  for (let i = 2; i < n; i++) {
    if ( n % i == 0) return false;
  }
  return true;
}
```

## 함수 표현식

[https://ko.javascript.info/function-expressions](https://ko.javascript.info/function-expressions)

자바스크립트는 함수를 특별한 종류의 값으로 취급하지 않는다(다른 언어에서처럼 "특별한 동작을 하는 구조"로 취급되지 않는다).

자바스크립트에서는 *함수 선언(Function Declaraion)* 방식 외에도 *함수 표현식(Function Expression)*을 사용해서 함수를 만들 수 있다.

```js
let sayHi = function() {
  alert( "Hello" );
};

// 즉...

function sayHi() {   // (1) 함수 생성
  alert( "Hello" );
}

let func = sayHi;    // (2) 함수 복사

func(); // Hello     // (3) 복사한 함수를 실행(정상적으로 실행됩니다)!
sayHi(); // Hello    //     본래 함수도 정상적으로 실행됩니다.
```

함수를 호출하기 위해선 괄호와 함께 함수 이름을 써야 한다.

```js
function sayHi() {
  alert( "Hello" );
}

alert( sayHi ); // 함수 코드가 보임
alert( sayHi() ); // Hello
```

* 함수 표현식의 끝에는 세미콜론이 붙어있다. 왜냐하면 구문 안에서 값의 역할을 하기 때문이다. 모든 구문의 끝에는 세미콜론이 붙는다. `if`, `for`, `function`의 중괄호에는 세미콜론이 코드 블록 끝에 붙지 않아도 된다.

### 콜백 함수

콜백 함수는 함수의 인수로 함수를 전달하고, 인수로 전달한 함수를 "나중에 호출(called back)"한다.

```js
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}

function showOk() {
  alert( "동의하셨습니다." );
}

function showCancel() {
  alert( "취소 버튼을 누르셨습니다." );
}

// 사용법: 함수 showOk와 showCancel가 ask 함수의 인수로 전달됨
ask("동의하십니까?", showOk, showCancel);

// 혹은 익명 함수(anonymous function)을 사용한다면...

function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}

ask(
  "동의하십니까?",
  function() { alert("동의하셨습니다."); },
  function() { alert("취소 버튼을 누르셨습니다."); }
);
```

콜백 함수는 자바스크립트의 정신을 대변한다.

### 함수 표현식 vs 함수 선언문

* 함수 표현식은 실제 실행 흐름이 해당 함수에 도달했을 때 함수를 생성한다. 따라서 실행 흐름이 함수에 도달했을 때부터 해당 함수를 사용할 수 있다.
* 반면 함수 선언문은 함수 선언문이 정의되기 전에도 호출할 수 있다. 따라서 전역 함수 선언문은 스크립트에서의 이취에 상관 없이 어디에서든 사용할 수 있다. 함수 선언문은 스크립트 실행 준비 단계에서 생성되기 때문이다.

```js
sayHi("John"); // Hello, John

function sayHi(name) {
  alert( `Hello, ${name}` );
}
```

* 엄격 모드에서 코드 블록 내에 위치한 함수 선언문은 블록 밖에서 접근하지 못한다.
* 하지만 함수 표현식을 사용하면 블록 밖에서도 사용할 수 있다.

```js
let age = prompt("나이를 알려주세요.", 18);

// 조건에 따라 함수를 선언함
if (age < 18) {

  function welcome() {
    alert("안녕!");
  }

} else {

  function welcome() {
    alert("안녕하세요!");
  }

}

// 함수를 나중에 호출합니다.
welcome(); // Error: welcome is not defined

// 하지만...

et age = prompt("나이를 알려주세요.", 18);

let welcome;

if (age < 18) {

  welcome = function() {
    alert("안녕!");
  };

} else {

  welcome = function() {
    alert("안녕하세요!");
  };

}

welcome(); // 제대로 동작합니다.
```

## 화살표 함수 기본

[https://ko.javascript.info/arrow-functions-basics](https://ko.javascript.info/arrow-functions-basics)

화살표 함수(arrow function)의 화살표는 우측의 `표현식(expression)`을 평가하고, 평가 결과를 반환한다.

```js
let sum = (a, b) => a + b;

/* 위 화살표 함수는 아래 함수의 축약 버전입니다.

let sum = function(a, b) {
  return a + b;
};
*/

alert( sum(1, 2) ); // 3
```

화살표 함수에 인수가 하나도 없을 땐 괄호를 비워놓으면 된다.

```js
let sayHi = () => alert("안녕하세요!");

sayHi();
```

화살표 함수에도 여러 줄의 본문이 들어갈 수 있다. 이 경우, 반드시 `return` 지시자를 사용해 반환 값을 명기해 주어야 한다.

```js
let sum = (a, b) => {  // 중괄호는 본문 여러 줄로 구성되어 있음을 알려줍니다.
  let result = a + b;
  return result; // 중괄호를 사용했다면, return 지시자로 결괏값을 반환해주어야 합니다.
};

alert( sum(1, 2) ); // 3
```