# Tuesday, June 21, 2022

[[Journal]]
[[javascript]]

## 배열

[https://ko.javascript.info/array](https://ko.javascript.info/array)

키를 사용할 때에는 객체를 이용한다. *순서가 있는 컬렉션*이 필요할 땐, 배열을 사용할 수 있다.

배열은 아래와 같이 선언한다:

```js
let arr = new Array();
let arr = [];

let fruits = ["사과", "오렌지", "자두"];
```

인덱싱을 사용하여 자료를 가져오거나 자료를 수정할 수 있다.

배열의 길이는 `배열.length`를 사용한다.

배열 요소의 자료형엔 제약이 없다.

### pop, push와 shift, unshift

* `push`: 배열의 맨 끝에 요소를 추가한다.
* `pop`: 배열의 끝 요소를 추출한다.
* `shift`: 배열의 제일 앞 요소를 꺼내 제거한 후, 남아있는 요소들을 앞으로 민다.
* `unshift`: 배열의 제일 앞에 요소를 추가한다.

`shift`, `unshift`는 배열의 맨 앞을 다루기에 속도가 느리다.

### 배열의 내부 동작 원리

배열은 본질적으로 객체이다. 자바스크립트 엔진은 배열의 요소를 인접한 메모리 공간에 차례로 저장한다.

### 반복문

배열을 순회하는 방법:

```js
// 인덱스
let arr = ["사과", "오렌지", "배"];

for (let i = 0; i < arr.length; i++) {
  alert( arr[i] );
}

// 순회 문법
let fruits = ["사과", "오렌지", "자두"];

// 배열 요소를 대상으로 반복 작업을 수행합니다.
for (let fruit of fruits) {
  alert( fruit );
}

// 객체 문법

let arr = ["사과", "오렌지", "배"];

for (let key in arr) {
  alert( arr[key] ); // 사과, 오렌지, 배
}
```

하지만 `for..in` 반복문은 객체 전용이기에 배열에 사용하면 객체에 사용하는 것 대비 10~100배 느리다! 따라서 배열엔 되도록 `for..in`을 사용하지 말자.

### `length` 프로퍼티

`length` 프로퍼티를 이용해 배열을 자를 수 있다.

```js
let arr = [1, 2, 3, 4, 5];

arr.length = 2; // 요소 2개만 남기고 잘라봅시다.
alert( arr ); // [1, 2]

arr.length = 5; // 본래 길이로 되돌려 봅시다.
alert( arr[3] ); // undefined: 삭제된 기존 요소들이 복구되지 않습니다.
```

### toString

배열엔 `toString` 메서드가 구현되어 있어 이를 호출하면 요소를 쉼표로 구분한 문자열이 반환된다.

```js
let arr = [1, 2, 3];

alert( arr ); // 1,2,3
alert( String(arr) === '1,2,3' ); // true
```
