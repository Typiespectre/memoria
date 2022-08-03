# Wednesday, August 3, 2022

[[Journal]]
[[javascript]]
[[nodejs]]

## Node JS 메모

tkim님 노션: [https://devtkim42.notion.site/nodejs-f95a0be6641f4e578bce2564478db759](https://devtkim42.notion.site/nodejs-f95a0be6641f4e578bce2564478db759)

### Express란?

Node.js를 위한 웹 프레임워크

- Node.js : 자바스크립트로 서버가 돌아가도록 하게 하는 플랫폼(환경).
- Node.js는 **짧은 시간에 대량의 클라이언트 요청을 처리하는 경우**에 적합하다.
- 반면, Node.js는 CPU의 사용이 높은 어플리케이션에는 적합하지 않음.

Express.js는 Node.js의 핵심모듈 HTTP와 Connect 컴포넌트를 기반으로 하는 웹프레임워크.
그러한 컴포넌트를 미들웨어(middleWare)라고 함. 미들웨어 덕분에 개바을 빠르고 손쉽게 할 수 있다.

참고: [https://koras02.tistory.com/49?category=967574](https://koras02.tistory.com/49?category=967574)

### 미들웨어

`const app = express()` 로 생성된 어플리케이션 오브젝트에는 `use()`라는 함수가 존재한다.
`use()` 함수를 이용하여 미들웨어를 등록, 순서대로 실행한다.
내부적으로 미들웨어는 `next`라는 매개변수를 받아, 자신의 함수 실행이 끝나면 `next()`로 다음 등록된 미들웨어 한수를 호출하여 **순서대로** 실행이 된다.

```js
app.use(express.json())
app.use(express.urlencoded({ extended: false }))
...
```

미들웨어 함수를 'callback' 이라고 부르는 것 같기도 하다.

참고: [https://morian-kim.tistory.com/2?category=854721](https://morian-kim.tistory.com/2?category=854721)

### `app.use`의 사용법

`app.use`는 미들웨어 설정을 하는 것 같음.

```js
// 모든 요청에 cookieParser()를 실행
app.use(cookieParser())
```

```js
// 직접 간단하게 미들웨어 만들기
app.use('/', (req, res, next) => {
    console.log('모든 요청에 log 남기기');
    next();
});
```

참고: [https://morian-kim.tistory.com/3](https://morian-kim.tistory.com/3)

### 왠만한 비교에는 `===`를 사용할 것!

### AND, OR 연산자의 반환값

`&&` 연산자는 본인(조건)이 참이면 오른쪽으로 넘어가 다른 녀석을 return하고, 본인이 거짓이면 본인을 그대로 return한다.
반대로 `||` 연산자는 본인이 참이면 본인을 그대로 return하고, 본인이 거짓이면 오른쪽으로 넘어가 다른 녀셕을 return한다.

```js
const result1 = true && '참'
console.log(result1); // "참"

const result2 = false || '거짓'
console.log(result2); // '거짓'

const result3 = true || '거짓'
console.log(result3); // true
```

### NVM vs. YARN

- yarn은 작업을 병렬로 처리하기에 npm보다 빠르다.
- npm은 보안 시스템에 취약성이 발생할 수 있음.

참고: [https://developer0809.tistory.com/128](https://developer0809.tistory.com/128)

### morgan

morgan은 HTTP 요총에 대한 로그를 남기는 미들웨어.
내맘대로 로그를 남길 수도 있슴.
`combined`는 로그 형식을 의미함.
프로젝트에서 `winston`도 같이 사용한다.

### `import`

`import()`는 사용자가 필요한 모듈 부분만 선택하고 로드할 수 있다. 따라서 `require()`보다 성능이 우수하고 메모리를 절약할 수 있다.

### `export`

`export`는 내부 스크립트 객체를 외부 스크립트로 모듈화하는 것이다. `export`를 선언하지 않으면 외부 스크립트에서 `import`를 통해 사용할 수 없다.

`export`는 스크립트 내 모든 객체에 선언할 수 있다.

named export 방식:

```js
export const exString = 'string';
export const exArray = [1, 2, 3];
export const exObject = {object: 'object'};
export function consoleFunction(target) {
    console.log(target);
}
export {consoleFunction as conFunc, exString, exArray, exObject}; // 한 번에 선언
```

default export 방식:

```js
export default consoleFunction; // 단일 선언
export {consoleFunction as default, exString, exArray, exObject}; // 다른 객체와 같이 선언
```

`default`는 한 객체에만 선언할 수 있기에 해당 스크립트의 메인 객체에만 선언하는 것이 좋다.
`default`는 `const`, `let`, `var`와 함께 선언하지 못한다.
