# Thursday, June 30, 2022

[[Journal]]
[[javascript]]

## interble 객체

배열, 문자열 모두 반복문을 사용할 수 있는 이터러블이다. 배열이 아닌 객체도 이터러블로 만든다면 순회(`for..of`)를 할 수 있다.

`Symbol.iterator` 메서드를 객체에 추가하면 이터러블로 작동하게 할 수 있다. 이터러블은 `next()` 메서드를 호출할 수 있어야 한다.

```js
// 이터러블이 아닌 객체.
let range = {
  from: 1,
  to: 5
};

// 1. for..of 최초 호출 시, Symbol.iterator가 호출됩니다.
range[Symbol.iterator] = function() {

  // Symbol.iterator는 이터레이터 객체를 반환합니다.
  // 2. 이후 for..of는 반환된 이터레이터 객체만을 대상으로 동작하는데, 이때 다음 값도 정해집니다.
  return {
    current: this.from,
    last: this.to,

    // 3. for..of 반복문에 의해 반복마다 next()가 호출됩니다.
    next() {
      // 4. next()는 값을 객체 {done:.., value :...}형태로 반환해야 합니다.
      if (this.current <= this.last) {
        return { done: false, value: this.current++ };
      } else {
        return { done: true };
      }
    }
  };
};

// 이제 의도한 대로 동작합니다!
for (let num of range) {
  alert(num); // 1, then 2, 3, 4, 5
}
```

혹은 아예 객체에 이터러블을 설정할 수도 있다.

```js
let range = {
  from: 1,
  to: 5,

  [Symbol.iterator]() {
    this.current = this.from;
    return this;
  },

  next() {
    if (this.current <= this.to) {
      return { done: false, value: this.current++ };
    } else {
      return { done: true };
    }
  }
};

for (let num of range) {
  alert(num); // 1, then 2, 3, 4, 5
}
```

### 이터레이터를 명시적으로 호출하기

```js
let str = "Hello";

// for..of를 사용한 것과 동일한 작업을 합니다.
// for (let char of str) alert(char);

let iterator = str[Symbol.iterator]();

while (true) {
  let result = iterator.next();
  if (result.done) break;
  alert(result.value); // 글자가 하나씩 출력됩니다.
}
```

### Array.from

이터러블과 유사 배열은 대개 배열이 아니기에 `push`, `pop`, `length` 등의 메서드를 지원하지 않지만, `Array.from` 메서드를 사용하면 배열처럼 작동하게 할 수 있다.

```js
let arrayLike = {
  0: "Hello",
  1: "World",
  length: 2
};

let arr = Array.from(arrayLike); // (*)
alert(arr.pop()); // World (메서드가 제대로 동작합니다.)
```

`Array.from`은 서로게이트 쌍을 처리할 수 있는 `slice`를 직접 구현할 수 있다.

```js
function slice(str, start, end) {
  return Array.from(str).slice(start, end).join('');
}

let str = '𝒳😂𩷶';

alert( slice(str, 1, 3) ); // 😂𩷶

// 내장 순수 메서드는 서로게이트 쌍을 지원하지 않습니다.
alert( str.slice(1, 3) ); // 쓰레깃값 출력 (영역이 다른 특수 값)
```
