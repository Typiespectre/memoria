# Sunday, June 26, 2022

[[Journal]]
[[javascript]]

## 배열과 메서드

[https://ko.javascript.info/array-methods](https://ko.javascript.info/array-methods)

객체 요소 추가/제거 메서드:  

* `arr.push(...items)` : 맨 끝에 요소 추가
* `arr.pop()` : 맨 끝 요소 제거
* `arr.shift()` : 맨 앞 요소 제거
* `arr.unshift(...items)` : 맨 앞에 요소 추가

### splice

`delete` 연산자는 배열을 이상하게 지운다.

```js
let arr = ["I", "go", "home"];

delete arr[1]; // "go"를 삭제합니다.

alert( arr[1] ); // undefined

// delete를 써서 요소를 지우고 난 후 배열 --> arr = ["I",  , "home"];
alert( arr.length ); // 3
```

`splice`라는 만능 메서드를 사용하자.

```js
arr.splice(index[, deleteCount, elem1, ..., elemN])
```

* `index` : 기준점
* `deleteCount` : 제거하고자 하는 요소의 개수
* `elem ...` : 배열에 추가할 요소

```js
let arr = ["I", "study", "JavaScript"];

arr.splice(1, 1); // 인덱스 1부터 요소 한 개를 제거

alert( arr ); // ["I", "JavaScript"]
```

```js
let arr = ["I", "study", "JavaScript", "right", "now"];

// 처음(0) 세 개(3)의 요소를 지우고, 이 자리를 다른 요소로 대체합니다.
arr.splice(0, 3, "Let's", "dance");

alert( arr ) // now ["Let's", "dance", "right", "now"]
```

`splice`는 반환 배열을 가질 수 있다.

```js
let arr = ["I", "study", "JavaScript", "right", "now"];

// 처음 두 개의 요소를 삭제함
let removed = arr.splice(0, 2);

alert( removed ); // "I", "study" <-- 삭제된 요소로 구성된 배열
```

요소를 제거하지 않고 요소를 추가하려면, `deleteCount`를 `0`으로 설정하면 된다.

### slice

`start` 인덱스부터 `end` 인덱스까지 요소를 복사한 새로운 배열을 반환한다.

```js
arr.slice([start], [end])
```

```js
let arr = ["t", "e", "s", "t"];

alert( arr.slice(1, 3) ); // e,s (인덱스가 1인 요소부터 인덱스가 3인 요소까지를 복사(인덱스가 3인 요소는 제외))

alert( arr.slice(-2) ); // s,t (인덱스가 -2인 요소부터 제일 끝 요소까지를 복사)
```

### concat

기존 배열 요소를 추가할 때 사용이 가능하다.

```js
let arr = [1, 2];

// arr의 요소 모두와 [3,4]의 요소 모두를 한데 모은 새로운 배열이 만들어집니다.
alert( arr.concat([3, 4]) ); // 1,2,3,4

// arr의 요소 모두와 [3,4]의 요소 모두, [5,6]의 요소 모두를 모은 새로운 배열이 만들어집니다.
alert( arr.concat([3, 4], [5, 6]) ); // 1,2,3,4,5,6

// arr의 요소 모두와 [3,4]의 요소 모두, 5와 6을 한데 모은 새로운 배열이 만들어집니다.
alert( arr.concat([3, 4], 5, 6) ); // 1,2,3,4,5,6
```

`concat`에 객체를 넣는 방법은 본문을 확인.

### forEach로 반복작업 하기

`forEach`로 배열 요소 각각에 함수를 실행할 수 있다.

```js
["Bilbo", "Gandalf", "Nazgul"].forEach((item, index, array) => {
  alert(`${item} is at index ${index} in ${array}`);
});
```

### 배열 탐색하기

* `indexOf(item, from)` : 인덱스 `from`부터 시작해 `item(요소)`를 찾는다. 요소를 발견하면 해당 요소의 인덱스를 반환하고, 그렇지 않으면 `-1`을 반환한다.
* `lastIndexOf(item, from)` : 위와 동일하지마 검색을 끝에서부터 시작한다.
* `includes(item, from)` : 인덱스 `from`부터 시작해 `item`를 찾는다. 요소를 발견하면 `true`를 반환한다.

위 메서드는 요소를 비교할 때 완전 항등 연산자 `===`를 사용한다. 따라서 `false`를 검색하면 `0`이 아닌 `false`를 찾는다!

```js
let arr = [1, 0, false];

alert( arr.indexOf(0) ); // 1
alert( arr.indexOf(false) ); // 2
alert( arr.indexOf(null) ); // -1

alert( arr.includes(1) ); // true
```

요소의 배열 존재 여부를 검사할 땐 `includes`를 사용하는 것이 좋다. `NaN`까지 확인할 수 있기 때문이다.

```js
const arr = [NaN];
alert( arr.indexOf(NaN) ); // -1 (완전 항등 비교 === 는 NaN엔 동작하지 않으므로 0이 출력되지 않습니다.)
alert( arr.includes(NaN) );// true (NaN의 여부를 확인하였습니다.)
```

### find와 findIndex

객체로 구성된 배열을 다룰 때 아주 꿀인 메서드. 실무에서도 많이 쓰인다고...

```js
let result = arr.find(function(item, index, array) {
  // true가 반환되면 반복이 멈추고 해당 요소를 반환합니다.
  // 조건에 해당하는 요소가 없으면 undefined를 반환합니다.
});
```

`index`, `array`는 잘 사용되지 않는 인자라고 한다.

```js
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

let user = users.find(item => item.id == 1);

alert(user.name); // John
```

`findIndex`는 `find`와 동일하나, 조건에 맞는 요소를 반환하는 대신 해당 요소의 인덱스를 반환한다. 조건에 맞는 요소가 없으면 `-1`이 반환된다.

### filter

`find`는 조건에 맞는 요소를 하나만 반환하는데, `filter`는 모두 찾아 배열로 반환한다!

```js
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

// 앞쪽 사용자 두 명을 반환합니다.
let someUsers = users.filter(item => item.id < 3);

alert(someUsers.length); // 2
```

### map

`map`은 배열 요소 전체를 대상으로 함수를 호출하고, 함수 호출 결과를 배열로 반환한다.

```js
let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length);
alert(lengths); // 5,7,6
```

### sort(fn)

`sort`는 배열의 요소를 정렬하며, **배열 자체**가 변경된다. 다만, 배열 안의 요소는 문자열로 취급되어 재 정렬이 된다. 따라서 `sort`에 새로운 함수를 넘겨주어야 의도한 정렬 결과가 나올 수 있다.

```js
function compareNumeric(a, b) {
  if (a > b) return 1;
  if (a == b) return 0;
  if (a < b) return -1;
}

let arr = [ 1, 2, 15 ];

arr.sort(compareNumeric);

alert(arr);  // 1, 2, 15
```

정렬 함수의 반환값의 경우, 첫 번쨰 인수가 두 번째 인수보다 크면 양수를 반환, 그렇지 않으면 음수를 반환하므로 아래와 같이 간단하게 만들 수 있다.

```js
let arr = [ 1, 2, 15 ];

arr.sort(function(a, b) { return a - b; });
// 화살표 함수 사용
arr.sort( (a, b) => a - b );

alert(arr);  // 1, 2, 15
```

문자열에는 `localCompare`을 사용하자.

```js
let countries = ['Österreich', 'Andorra', 'Vietnam'];

alert( countries.sort( (a, b) => a > b ? 1 : -1) ); // Andorra, Vietnam, Österreich (제대로 정렬이 되지 않았습니다.)

alert( countries.sort( (a, b) => a.localeCompare(b) ) ); // Andorra,Österreich,Vietnam (제대로 정렬되었네요!)
```

### reverse

`reverse`는 배열의 요소를 역순으로 정렬한다.

### split과 join

`split`은 구분자를 기준으로 문자열을 쪼개어 배열로 반환한다. 메서드의 두 번째 인수에 숫자를 넘겨주면 배열의 길이를 제한하여 수행할 수 있다.

```js
let arr = 'Bilbo, Gandalf, Nazgul, Saruman'.split(', ', 2);

alert(arr); // Bilbo, Gandalf
```

`join`은 배열 요소를 모두 합쳐 하나의 문자열을 반환한다.

```js
let arr = ['Bilbo', 'Gandalf', 'Nazgul'];

let str = arr.join(';'); // 배열 요소 모두를 ;를 사용해 하나의 문자열로 합칩니다.

alert( str ); // Bilbo;Gandalf;Nazgul
```

### reduce와 reduceRight

`forEach`, `for`, `for..of`를 사용하면 배열을 순회하며 작업을 수행할 수 있다.
동일하게 배열을 순회하며 작업을 수행한 후, 작업 결과물을 새로운 배열의 형태로 얻으려면 `map`을 사용한다.

`reduce`는 이전 함수 호출 결과를 다음 함수 호출할 때 인수로 삼을 수 있다.

```js
let value = arr.reduce(function(accumulator, item, index, array) {
  // ...
}, [initial]);
```

* `accumulator` : 이전 함수 호출의 결과
* `initial` : 함수 최초 호출 시 사용되는 초깃값(옵션)
* `item` : 현재 배열 요소
* `index` : 요소의 위치
* `array` : 배열

일반적으로 아래와 같은 누산기처럼 사용이 된다:

```js
let arr = [1, 2, 3, 4, 5];

let result = arr.reduce((sum, current) => sum + current, 0);

alert(result); // 15
```

`current`에 배열의 요소가 하나씩 들어가는 방식이다. 뒤의 초깃값을 생랼할 수도 있지만, 이 경우 배열이 비어있는 상태라면 에러가 발생한다. `reduceRight`는 배열의 오른쪽부터 연산을 수행한다.

### isArray

`isArray`로 해당 객체가 배열인지 검사할 수 있다.

`sort`, `reverse`, `splice`는 기존 배열을 변형시킨다.