# Thursday, June 2, 2022

[[Journal]]
[[Javascript]]

## Hello, World!

[https://ko.javascript.info/hello-world](https://ko.javascript.info/hello-world)

`<script>` 태그에는 자바스크립트 코드가 들어가고, 코드가 자동으로 처리된다.

크기가 큰 자바스크립트 코드는 `src` 속성을 이용하여 HTML에 삽입할 수 있다.

```js
<script src="/path/to/script.js"></script>
```

`src` 속성이 있으면, 태그 내부의 코드는 무시된다.

```js
<script src="file.js">
  alert(1); // src 속성이 사용되었으므로 이 코드는 무시됩니다.
</script>
```

## 코드 구조

[https://ko.javascript.info/structure](https://ko.javascript.info/structure)

일반적으로 줄 바꿈이 있다면 세미콜론을 생략할 수 있다(자바스크립트는 줄 바꿈이 있으면 이를 '암시적' 세미콜론으로 해석한다). 하지만 모든 경우에 그러한 것은 아니다!

```js
alert("에러가 발생합니다.")

[1, 2].forEach(alert)   // 오류 발생
```

가령, 자바스크립트는 대괄호 앞에 세미콜론이 있다고 가정하지 않기에, 위 코드는 문제가 발생한다. 따라서 문(statement) 사이엔 세미콜론을 넣는 것이 좋다(아니, 따라야 한다!).

자바스크립트의 주석은 `//`이다. 여러 줄 주석 처리를 하고자 한다면, `/* ... */`를 사용하면 된다. 다만 중첩 주석은 에러가 발생한다.

```js
/*
  /* 중첩 주석 ?!? */
*/
alert( 'World' );
```