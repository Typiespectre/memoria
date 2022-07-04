# Monday, July 4, 2022

[[Journal]]
[[javascript]]

## 맵과 셋

맵(Map)은 키가 있는 데이터를 저장한다는 점에서 객체와 유사하지만, 맵은 키에 다양한 자료형을 허용할 수 있다.

* `new Map()` : 맵을 만든다.
* `map.set(key, value)` : `key`를 이용해 `value`를 저장한다.
* `map.get(key)` : `key`에 해당하는 값을 반환합니다.
* `map.has(key)` : `key`가 존재하면 `true`, 존재하지 않으면 `false`를 반환한다.
* `map.delete(key)` : `key`에 해당하는 값을 삭제한다.
* `map.clear()` : 맵 안의 모든 요소를 제거한다.
* `map.size()` : 요소의 개수를 반환한다.

```js
let map = new Map();

map.set('1', 'str1');   // 문자형 키
map.set(1, 'num1');     // 숫자형 키
map.set(true, 'bool1'); // 불린형 키

// 객체는 키를 문자형으로 변환한다는 걸 기억하고 계신가요?
// 맵은 키의 타입을 변환시키지 않고 그대로 유지합니다. 따라서 아래의 코드는 출력되는 값이 다릅니다.
alert( map.get(1)   ); // 'num1'
alert( map.get('1') ); // 'str1'

alert( map.size ); // 3
```

`map`을 사용할 땐 `map[key]`가 아닌 `set`, `get`을 사용하자. 맵은 객체와 달리 키를 문자형으로 반환하지 않기 때문이다(즉 숫자 키의 경우, `map` 객체를 일반 객체로 취급이 될 수 있다).

**맵은 키로 객체를 허용한다.**

```js
let john = { name: "John" };

// 고객의 가게 방문 횟수를 세본다고 가정해 봅시다.
let visitsCountMap = new Map();

// john을 맵의 키로 사용하겠습니다.
visitsCountMap.set(john, 123);

alert( visitsCountMap.get(john) ); // 123
```

### 맵의 요소에 반복 작업하기

* `map.keys()` : 각 요소의 키를 모은 반복 가능한(이터러블) 객체를 반환한다.
* `map.values()` : 각 요소의 값을 모은 이터러블 객체를 반환한다.
* `map.entries()` : 요소의 `[키, 값]`을 한 쌍으로 하는 이터러블 객체를 반환한다. 이 이터러블 객체는 `for..of` 반복문의 기초로 쓰인다.

```js


let recipeMap = new Map([
  ['cucumber', 500],
  ['tomatoes', 350],
  ['onion',    50]
]);

// 키(vegetable)를 대상으로 순회합니다.
for (let vegetable of recipeMap.keys()) {
  alert(vegetable); // cucumber, tomatoes, onion
}

// 값(amount)을 대상으로 순회합니다.
for (let amount of recipeMap.values()) {
  alert(amount); // 500, 350, 50
}

// [키, 값] 쌍을 대상으로 순회합니다.
for (let entry of recipeMap) { // recipeMap.entries()와 동일합니다.
  alert(entry); // cucumber,500 ...
}
```

맵은 삽입된 순서대로 순회를 실시한다. 배열과 유사하게 `forEach`도 사용가능하다.

```js
// 각 (키, 값) 쌍을 대상으로 함수를 실행
recipeMap.forEach( (value, key, map) => {
  alert(`${key}: ${value}`); // cucumber: 500 ...
});
```

### `Object.entries`: 객체를 맵으로 바꾸기

`Object.entries(obj)`를 사용하면 객체를 맵으로 바꿔 사용할 수 있다.

```js
let obj = {
  name: "John",
  age: 30
};

let map = new Map(Object.entries(obj));

alert( map.get('name') ); // John
```

### `Object.fromEntries`: 맵을 객체로 바꾸기

반대로 `Object.fromEntries(obj)`를 사용하면 맵을 객체로 바꿀 수 있다.

```js
let map = new Map();
map.set('banana', 1);
map.set('orange', 2);
map.set('meat', 4);

let obj = Object.fromEntries(map.entries()); // 맵을 일반 객체로 변환 (*)

// 맵이 객체가 되었습니다!
// obj = { banana: 1, orange: 2, meat: 4 }

alert(obj.orange); // 2
```

### 셋

셋(Set)은 중복을 허용하지 않는 값을 모아놓은 특별한 컬렉션이다. 셋에는 키가 없는 값이 저장된다.

* `new Set(iterable)` : 셋을 만든다. 이터러블 객체를 전달받으면 그 안의 값을 복사해 셋에 넣는다.
* `set.add(value)` : 값을 추가하고 셋 자신을 반환한다.
* `set.delete(value)` : 값을 제거한다. 제거에 성공하면 `true`, 아니면 `false`를 반환한다.
* `set.has(value)` : 셋 내에 값이 존재하면 `true`, 아니면 `false`를 반환한다.
* `set.clear()` : 셋을 비운다.
* `set.size` : 셋에 몇 개의 값이 있는지 센다.

```js
let set = new Set();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

// 어떤 고객(john, mary)은 여러 번 방문할 수 있습니다.
set.add(john);
set.add(pete);
set.add(mary);
set.add(john);
set.add(mary);

// 셋에는 유일무이한 값만 저장됩니다.
alert( set.size ); // 3

for (let user of set) {
  alert(user.name); // // John, Pete, Mary 순으로 출력됩니다.
}
```

셋은 값의 유일무이함을 확인하는데 최적화되어있다.

### 셋의 값에 반복 작업하기

* `set.keys()` : 셋 내의 모든 값을 포함하는 이터러블 객체를 반환한다.
* `set.values()` : `set.keys()`와 동일한 작업을 한다. 맵과의 호환성을 위해 만들어진 메서드.
* `set.entries()` : 셋 내의 각 값을 이용해 만든 `[value, value]` 배열을 포함하는 이터러블 객체를 반환한다. 역시 맵과의 호환성을 위해 만들어진 메서드.

```js
let set = new Set(["oranges", "apples", "bananas"]);

for (let value of set) alert(value);

// forEach를 사용해도 동일하게 동작합니다.
set.forEach((value, valueAgain, set) => {
  alert(value);
});
```

`forEach`는 세 개의 인수를 받기에, 첫 번쨰 인수에 값을 저장, 두 번쨰 인수에도 같은 값을 저장, 그리고 마지막에는 목표하는 객체(셋)을 설정한다.

### 요약

중요한 점은, 맵과 셋은 값을 추가한 대로 내용이 설정된다. 따라서 정렬되어있지 않다. 값을 재정렬하거나 인덱싱을 사용하는 것은 불가능하다.