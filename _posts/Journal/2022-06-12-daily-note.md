# Sunday, June 12, 2022

[[Journal]]
[[javascript]]

## 객체

[https://ko.javascript.info/object](https://ko.javascript.info/object)

객체형은 다양한 데이터를 담을 수 있다. 객체는 중괄호 `{...}`를 이용해 만들 수 있다. 중괄호 안에는 '키(key):값(value)' 쌍으로 이루어진 *프로퍼티(property)*를 여러 개 넣을 수 있는데, '키'에는 문자형, '값'에는 모든 자료형이 허용된다. 빈 객체를 만드는 방법은 두 가지가 있다:

```js
let user = new Object(); // '객체 생성자' 문법
let user = {};  // '객체 리터럴' 문법. 주로 이 방법을 사용함
```

### 리터럴과 프로퍼티

```js
let user = {     // 객체
  name: "John",  // 키: "name",  값: "John"
  age: 30        // 키: "age", 값: 30
  "likes birds": true  // 복수의 단어는 따옴표로 묶어야 합니다.
};
```

점 표기법(dot notation)을 이용하면 프로퍼티 값을 읽을 수 있다.

```js
// 프로퍼티 값 얻기
alert( user.name ); // John
alert( user.age ); // 30
```

`delete` 연산자를 사용하면 프로퍼티를 삭제할 수 있다.

```js
delete user.age;
```

`const`로 선언된 객체는 수정될 수 있다.

```js
const user = {
  name: "John"
};

user.name = "Pete"; // (*)

alert(user.name); // Pete
```

### 대괄호 표기법

대솰호 표기법을 사용하면 코드를 유연하게 작성할 수 있다.

```js
let user = {
  name: "John",
  age: 30
};

let key = "name";
// 점 표기법은 이런 접근이 불가능함
alert( user.key ); // undefined
alert(user[key]); // John
```

### 계산된 프로퍼티

객체 리터럴 안의 프로퍼티가 대괄호로 둘러싸여 있는 경우를 *계산된 프로퍼티(computed property)*라고 부른다.

```js
let fruit = prompt("어떤 과일을 구매하시겠습니까?", "apple");

let bag = {
  [fruit]: 5, // 변수 fruit에서 프로퍼티 이름을 동적으로 받아 옵니다.
};

alert( bag.apple ); // fruit에 "apple"이 할당되었다면, 5가 출력됩니다.

// or

let fruit = prompt("어떤 과일을 구매하시겠습니까?", "apple");
let bag = {};

// 변수 fruit을 사용해 프로퍼티 이름을 만들었습니다.
bag[fruit] = 5;
```

### 단축 프로퍼티

```js
function makeUser(name, age) {
  return {
    name: name,
    age: age,
    // ...등등
  };
}

let user = makeUser("John", 30);
alert(user.name); // John
```

위를 아래처럼 줄일 수 있다:

```js
function makeUser(name, age) {
  return {
    name, // name: name 과 같음
    age,  // age: age 와 같음
    // ...
  };
}
```

## 프로퍼티 이름의 제약사항

변수 이름에는 예약어를 사용하면 안되지만, 객체 프로퍼티에는 그런 특정한 제약이 없다.

```js
// 예약어를 키로 사용해도 괜찮습니다.
let obj = {
  for: 1,
  let: 2,
  return: 3
};

alert( obj.for + obj.let + obj.return );  // 6
```

키에 숫자형을 넣어도 문자형으로 자동 변환된다.
```js
let obj = {
  0: "test" // "0": "test"와 동일합니다.
};

// 숫자 0은 문자열 "0"으로 변환되기 때문에 두 얼럿 창은 같은 프로퍼티에 접근합니다,
alert( obj["0"] ); // test
alert( obj[0] ); // test (동일한 프로퍼티)
```

`__proto__`를 사용하면 문자형으로 형변환이 되지 않는다.

```js
let obj = {};
obj.__proto__ = 5; // 숫자를 할당합니다.
alert(obj.__proto__); // [object Object] - 숫자를 할당했지만 값은 객체가 되었습니다.
```

### `in` 연산자로 프로퍼티 존재 여부 확인하기

```js
let user = { name: "John", age: 30 };

alert( "age" in user ); // user.age가 존재하므로 true가 출력됩니다.
alert( "blabla" in user ); // user.blabla는 존재하지 않기 때문에 false가 출력됩니다.
```

### `for...in` 반복문

`for...in`은 `for(;;)` 반복문과는 다르다.

```js
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for (let key in user) {
  // 키
  alert( key );  // name, age, isAdmin
  // 키에 해당하는 값
  alert( user[key] ); // John, 30, true
}
```

이 반복문은 파이썬과 비슷하군!

### 객체 정렬 방식

정수 프로퍼티(integer property)는 자동으로 정렬되고, 그 외의 프로퍼티는 객체에 추가한 순서 그대로 정렬된다.

위와 같은 일반 객체 외에도, `Array`, `Date`, `Error`와 같은 다양한 종류의 객체가 있다.

## 참조에 의한 객체 복사

[https://ko.javascript.info/object-copy](https://ko.javascript.info/object-copy)

객체가 선언된 변수에는 객체가 그대로 저장되는 것이 아니라, 객체가 저장되어있는 '메모리 주소'인 객체에 대한 '참조 값'이 저장된다.

### 객체 복사, 병합과 Object.assign

자바스크립트는 객체 내장 메서드를 지원하지 않는다. 복제가 필요한 상황이라면 새로운 객체를 만든 다음, 기존 객체의 프로퍼티들을 순회하면 된다.

```js
let user = {
  name: "John",
  age: 30
};

let clone = {}; // 새로운 빈 객체

// 빈 객체에 user 프로퍼티 전부를 복사해 넣습니다.
for (let key in user) {
  clone[key] = user[key];
}

// 이제 clone은 완전히 독립적인 복제본이 되었습니다.
clone.name = "Pete"; // clone의 데이터를 변경합니다.

alert( user.name ); // 기존 객체에는 여전히 John이 있습니다.
```

혹은 `Object.assign`을 사용할 수도 있다.

```js
Object.assign(dest, [src1, src2, src3...])
```

```js
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// permissions1과 permissions2의 프로퍼티를 user로 복사합니다.
// 동일한 이름을 가진 프로퍼티인 경우, 기존 값이 덮어씌워진다.
Object.assign(user, permissions1, permissions2, { name: "Pete" });

// now user = { name: "Pete", canView: true, canEdit: true }
```

아래처럼 반복문 없이 객체를 복사할 수 있다.

```js
let user = {
  name: "John",
  age: 30
};

let clone = Object.assign({}, user);
```

위와 같은 복사를 얕은 복사(shallow copy)라고 부른다.

### 깊은 복사

깊은 복사(deep copy)를 사용하려면, 자바스크립트 라이브러리 `lodash`의 메서드인 `_.cloneDeep(obj)`를 사용하거나, 깊은 복사 알고리즘을 사용할 것.

## 가비지 컬렉션

[https://ko.javascript.info/garbage-collection](https://ko.javascript.info/garbage-collection)

쓸모 없어진 변수와 같이, 더 이상 참조되지 않는 메모리를 수거하는 일을 가비지 컬렉션이라고 부른다.

### 가비지 컬렉션 기준

자바스크립트는 *도달 가능성(reachability)*라는 개념을 사용하여 메모리 관리를 수행한다. 도달 가능한 값, 즉 접근 가능하거나 사용할 수 있는 값은 메모리에서 삭제되지 않는다.

* 현재 함수의 지역 변수와 매개변수
* 중첩 함수의 체인에 있는 함수에서 사용되는 변수와 매개변수
* 전역 변수
* 기타 등등

위와 같은 값들은 태생부터 도달 가능하기에, 명백한 이유 없이는 삭제되지 않는다. 이런 값을 *루트(root)*라고 부른다.

또한 루트가 참조하는 값이나 체이닝으로 루프에서 참조할 수 있는 값도 도달 가능한 값이 된다.

자바스크립트 엔진 내에선 가비지 컬렉터가 끊임없이 동작한다.

자바스크립트의 알고리즘이나 최적화 기법은 링크를 참조하자!

## 메서드와 this

[https://ko.javascript.info/object-methods](https://ko.javascript.info/object-methods)

객체 프로퍼티에 할당된 함수를 *메서드(method)*라고 부른다. 객체를 사용하여 개체를 표현하는 방식을 '객체 지향 프로그래밍(object-oriented programming, OOP)'라고 부른다. 메서드를 사용하여, 객체가 특정한 *행동*을 할 수 있게 된다.

```js
let user = {
  name: "John",
  age: 30
};

user.sayHi = function() {
  alert("안녕하세요!");
};

user.sayHi(); // 안녕하세요!
```

### 메서드와 this

메서드는 객체에 저장된 정보에 접근을 할 수 있어야 제 역할을 할 수 있다. 대부분의 메서드가 객체 프로퍼티의 값을 활용한다.

**메서드 내부에서`this` 키워드를 사용하면 객체에 접근할 수 있다.**

```js
let user = {
  name: "John",
  age: 30,

  sayHi() {
    // 'this'는 '현재 객체'를 나타냅니다.
    alert(this.name);
  }
};

user.sayHi(); // John
```

외부변수를 사용할 수도 있지만(`user.name`), 만약 객체를 복사하는 경우, `user.name`이 그대로 옮겨가게 되어, 원치 않는 결과가 나오게 된다.

### 자유로운 this

자바스크립트에선 모든 함수에 `this`를 사용할 수 있다. `this`의 값은 런타임에 결정된다. 컨텍스트에 따라 달라진다.

함수 본문에 `this`가 사용되었다면, 객체 컨텍스트 내에서 함수를 호출할 것이라고 예상하면 된다.

```js
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// 별개의 객체에서 동일한 함수를 사용함
user.f = sayHi;
admin.f = sayHi;

// 'this'는 '점(.) 앞의' 객체를 참조하기 때문에
// this 값이 달라짐
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (점과 대괄호는 동일하게 동작함)
```

`this`는 '점 앞의' 객체가 무엇인가에 따라 자유롭게 결정된다.

### `this`가 없는 화살표 함수

화살표 함수는 일반 함수와는 달리 '고유한' `this`를 가지지 않는다. 화살표 함수에서 `this`를 참조하면, 화살표 함수가 아닌 '평범한' 외부 함수에서 `this`를 가져온다.

```js
let user = {
  firstName: "보라",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // 보라 
```

참고:

```js
function makeUser() {
  return {
    name: "John",
    ref() {
      return this;
    }
  };
};

let user = makeUser();

alert( user.ref().name ); // John
```

