# Monday, June 13, 2022

[[Journal]]
[[javascript]]

## new 연산자와 생성자 함수

[https://ko.javascript.info/constructor-new](https://ko.javascript.info/constructor-new)

`new` 연산자와 생성자 함수를 사용하면 유사한 객채 여러 개를 쉽게 만들 수 있다. 재사용할 수 있는 객체를 가능하게 한다.

### 생성자 함수

생성자 함수는 두 관례를 따른다:

1. 함수 이름의 첫 글자는 대문자로 시작한다.
2. 반드시 `new` 연산자를 붙여 실행된다.

```js
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

let user = new User("보라");

alert(user.name); // 보라
alert(user.isAdmin); // false
```

익명 생성자 함수는 재사용할 필요가 없는 복잡한 객체의 경우 사용할 수 있다:

```js
let user = new function() {
  this.name = "John";
  this.isAdmin = false;

  // 사용자 객체를 만들기 위한 여러 코드.
  // 지역 변수, 복잡한 로직, 구문 등의
  // 다양한 코드가 여기에 들어갑니다.
};
```

`new`가 붙어있으면 새로운 객체라는 사실을 쉽게 알 수 있기에, `new`를 생략해서 객체를 만드는 일은 최대한 피하자.

생성자 함수는 일반 함수이지만, 일반 함수와 구분하기 위해 이름 첫 글자를 대문자로 쓴다. 생성자 함수는 `new` 연산자와 함께 호출해야 하며, `new`가 호출되면 내부에서 `this`가 암시적으로 만들어지고, 마지막엔 `this`가 반환된다.

## 옵셔널 체이닝 `?.`

[https://ko.javascript.info/optional-chaining](https://ko.javascript.info/optional-chaining)

옵셔널 체이닝(optional chaining) `?.`을 사용하면 프로퍼티가 없는 중첩 객체를 에러 없이 안전하게 접근할 수 있다.

`?.`은 `?.` '앞'의 평가 대상이 `undefined`나 `null`이면 평가를 멈추고 `undefined`를 반환한다.

```js
let user = {}; // 주소 정보가 없는 사용자

alert( user?.address?.street ); // undefined, 에러가 발생하지 않습니다.
```

하지만 옵셔널 체이닝을 남용하면 디버깅이 어려워질 수 있다. 또한 옵셔널 체이닝 앞의 변수는 반드시 선언되어 있어야 한다.

`?.`는 왼쪽의 평가대상에 값이 없으면 즉시 평가를 멈춘다(단락 평가, short-circuit).

`?.`는 연산자가 아니다. 함수나 대괄호와 함께 동작하는 특별한 문법 구조체(sysntax construct)이다.

함수나 대괄호와 사용하는 방식은 아래와 같다:

```js
let user1 = {
  admin() {
    alert("관리자 계정입니다.");
  }
}

let user2 = {};

user1.admin?.(); // 관리자 계정입니다.
user2.admin?.();
```

```js
let user1 = {
  firstName: "Violet"
};

let user2 = null; // user2는 권한이 없는 사용자라고 가정해봅시다.

let key = "firstName";

alert( user1?.[key] ); // Violet
alert( user2?.[key] ); // undefined

alert( user1?.[key]?.something?.not?.existing); // undefined
```

* `obj?.[prop]` – obj가 존재하면 `obj[prop]`을 반환하고, 그렇지 않으면 `undefined`를 반환함
* `obj?.method()` – obj가 존재하면 `obj.method()`를 호출하고, 그렇지 않으면 `undefined`를 반환함

`?.`은 `delete`와 조합해 사용할 수도 있다.

```js
delete user?.name; // user가 존재하면 user.name을 삭제합니다.
```

## 심볼형

[https://ko.javascript.info/symbol](https://ko.javascript.info/symbol)

자바스크립트는 객체 프로퍼티 키로 오직 문자형과 심볼형만을 허용한다.

### 심볼

'심볼(symbol)'은 유일한 식별자(unique identifier)를 만들 때 사용한다. `Symbol()`을 사용하면 심볼값을 만들 수 있다. 심볼을 만들 때 심볼 이름이라는 설명을 붙이면 디버깅 시 아주 유용하다.

```js
// id는 새로운 심볼이 됩니다.
let id = Symbol();

// 심볼 id에는 "id"라는 설명이 붙습니다.
let id = Symbol("id");
```

심볼은 유일성이 보장되는 자료형이기에, 설명이 동일한 심볼을 여러개 만들어도 각 심볼값은 다르다.

### '숨김' 프로퍼티

심볼을 이용하면 '숨김(hidden)' 프로퍼티를 만들 수 있다. 숨김 프로퍼티는 외부 코드에서 접근이 불가능하고 값도 덮어쓸 수 없는 프로퍼티이다. 서드파티 코드에서 가지고 온 객체에 식별자를 부여하는 역할을 할 수 있다.

```js
let user = { // 서드파티 코드에서 가져온 객체
  name: "John"
};

let id = Symbol("id");

user[id] = 1;

alert( user[id] ); // 심볼을 키로 사용해 데이터에 접근할 수 있습니다.
```

객체에 심볼형 키를 만들고자 한다면 대괄호를 사용해야 한다.

```js
let id = Symbol("id");

let user = {
  name: "John",
  [id]: 123 // "id": 123은 안됨
};
```

심볼 프로퍼티는 `for...in` 반복문으로 확인할 수 없다!

```js
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123
};

for (let key in user) alert(key); // name과 age만 출력되고, 심볼은 출력되지 않습니다.

// 심볼로 직접 접근하면 잘 작동합니다.
alert( "직접 접근한 값: " + user[id] );
```

`Object.assign`은 심볼 키까지 모두 복사한다.

이름이 같은 심볼이 같은 개체를 가리키게 하고자 한다면, 전역 심볼을 사용하면 된다. *전역 심볼 레지스트리(global symbol registry)* 안에 있는 심볼을 읽거나 새로운 심볼을 생성하려면 `Symbol.for(key)`를 사용하면 된다.

```js
// 전역 레지스트리에서 심볼을 읽습니다.
let id = Symbol.for("id"); // 심볼이 존재하지 않으면 새로운 심볼을 만듭니다.

// 동일한 이름을 이용해 심볼을 다시 읽습니다(좀 더 멀리 떨어진 코드에서도 가능합니다).
let idAgain = Symbol.for("id");

// 두 심볼은 같습니다.
alert( id === idAgain ); // true
```

전역 심볼의 이름을 찾기 위해선 `Symbol.keyFor(sym)`을 사용하면 된다. 일반 심볼에서 이름을 얻고 싶으면 `description` 프로퍼티를 사용하면 된다.

```js
let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");

alert( Symbol.keyFor(globalSymbol) ); // name, 전역 심볼
alert( Symbol.keyFor(localSymbol) ); // undefined, 전역 심볼이 아님

alert( localSymbol.description ); // name
```

## 객체를 원시형으로 변환하기

[https://ko.javascript.info/object-toprimitive](https://ko.javascript.info/object-toprimitive)

객체 연산을 하는 경우, 자동으로 형 변환이 일어난다. 객체는 원시값으로 변환되고, 이후 의도한 연산이 수행된다.

* 객체는 논리 평가시 `true`를 반드시 반환한다.
* 숫자형으로의 형 변환은 객체끼리 빼는 연산을 할 때나 수학 관련 함수를 적용할 떄 일어난다.
* 문자형으로의 형 변환은 대개 `alert(obj)` 같이 객체 출력의 경우 일어난다.

```js
let user = {
  name: "John",
  money: 1000,

  [Symbol.toPrimitive](hint) {
    alert(`hint: ${hint}`);
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  }
};

// 데모:
alert(user); // hint: string -> {name: "John"}
alert(+user); // hint: number -> 1000
alert(user + 500); // hint: default -> 1500
```

'hint'는 '목표로 하는 자료형'을 의미한다고 이해하면 될 것.

`toString`과 `valueOf`를 사용하면 형 변환을 직접 구현할 수 있다.