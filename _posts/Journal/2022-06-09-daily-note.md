# Thursday, June 9, 2022

[[Journal]]
[[javascript]]

## Chrome으로 디버깅하기

[https://ko.javascript.info/debugging-chrome](https://ko.javascript.info/debugging-chrome)

## 코딩 스타일

[https://ko.javascript.info/coding-style](https://ko.javascript.info/coding-style)

많은 자바스크립트 코드는 이집션(Egyptian) 스타일을 가진다. 물론 코드 스타일은 개발자의 자유이다!

```js
// Egyptian 스타일 코드
if (condition) {
  // 코드 1
  // 코드 2
  // ...코드 n...
}
```

하지만 아래와 같은 코드는 절대 사용하지 말자. 오류가 발생한다.

```js
if (n < 0) 
    alert(`Power ${n} is not supported`);
```

가로 들여쓰기: 스페이스 두 개 혹은 네 개를 많이 사용한다.
세로 들여쓰기: 변수 선언, 반복문, 리턴문 사이에 빈 줄을 넣어 가독성을 높인다.

세미콜론을 자주 사용하자!

중첩 레벨을 최대한 줄이자.

Linter를 적극적으로 사용하자!

## 주석

[https://ko.javascript.info/comments](https://ko.javascript.info/comments)

좋은 코드에는 '설명이 담긴(explanatory)' 주석이 많아선 안된다. 주석 없이 코드 자체만으로 코드가 무슨 일을 하는지 쉽게 이해할 수 있어야 한다.

함수 이름 자체가 주석의 역할을 하는 코드: *자기 설명적인(self-descriptive)* 코드

그렇다면, 좋은 주석이란?

* 아키텍처를 설명하는 주석: 조감도 역할을 한다. UML.
* 함수 용례와 매개변수 정보를 담고 있는 주석

```js
/**
 * x를 n번 곱한 수를 반환함
 *
 * @param {number} x 거듭제곱할 숫자
 * @param {number} n 곱할 횟수, 반드시 자연수여야 함
 * @return {number} x의 n 거듭제곱을 반환함
 */
function pow(x, n) {
  ...
}
```

* 왜 이런 방법으로 문제를 해결했는지를 설명하는 주석: 이전에 했던 실수를 방지하는 안내판 역할
* 미묘한 기능이 있고, 이 기능이 어디에 쓰이는지를 설명하는 주석

## 닌자 코드

[https://ko.javascript.info/ninja-code](https://ko.javascript.info/ninja-code)

## 테스트 자동화와 Mocha

[https://ko.javascript.info/testing-mocha](https://ko.javascript.info/testing-mocha)

수동으로 테스트를 진행하는 일은 불완전하다.

## Behavior Driven Development

BDD는 테스트(test), 문서(documentation), 예시(example)를 한데 모아놓은 개념이다.

## 개발 순서

BDD 방법론을 이용한 개발 순서는 다음과 같다:

1. 명세서 초안을 작성합니다. 초안엔 기본적인 테스트도 들어갑니다.
2. 명세서 초안을 보고 코드를 작성합니다.
3. 코드가 작동하는지 확인하기 위해 Mocha라 불리는 테스트 프레임워크를 사용해 명세서를 실행합니다. 이때, 코드가 잘못 작성되었다면 에러가 출력됩니다. 개발자는 테스트를 모두 통과해 에러가 더는 출력되지 않을 때까지 코드를 수정합니다.
4. 모든 테스트를 통과하는 코드 초안이 완성되었습니다.
5. 명세서에 지금까진 고려하지 않았던 유스케이스 몇 가지를 추가합니다. 테스트가 실패하기 시작할 겁니다.
6. 세 번째 단계로 돌아가 테스트를 모두 통과할 때까지 코드를 수정합니다.
7. 기능이 완성될 때까지 3~6단계를 반복합니다.

*명세서(specification)*, 짧게 줄여 *스펙(spec)*은 아래와 같은 형태를 가진다:

```js
describe("pow", function() {

  it("주어진 숫자의 n 제곱", function() {
    assert.equal(pow(2, 3), 8);
  });

});
```

* `describe("title", function() { ... })` : 구현하고자 하는 기능에 대한 설명이 들어간다.
* `it("유스 케이스 설명", function() { ... })` : 설명은 자연어로 작성되어야 한다. 두 번째 인수에는 테스트 함수가 들어간다.
* `assert.equal(value1, value2)` : `it` 블록 내의 코드가 에러 없이 실행되어야 한다.

## 스펙 실행하기

Mocha, Chai, Sinon이라는 라이브러리를 사용한다. 자세한 구현 사항은 링크를 확인하기!

잘 테스트 된 코드는 더 나은 아키텍처를 만든다.

## 폴리필

[https://ko.javascript.info/polyfills](https://ko.javascript.info/polyfills)

엔진이 반드시 모든 기능을 지원하는 것은 아니다. 즉 명세서 내 모든 기능이 엔진에 구현되어있지 않을 수 있다.

폴리필(Polyfill): 브라우저에서 지원하지 않는 코드를 사용 가능한 코드 조각이나 플러그인으로 변환한 코드를 의미한다. 즉, 최신 자바스크립트의 기능을 구식 자바스크립트 코드로 똑같이 구현한 코드를 의미함.

### 바벨

위와 같은 상황에서 바벨을 사용할 수 있다. 바벨은 모던 자바스크립트 코드를 구 표준을 준수하는 코드로 바꿔준다.