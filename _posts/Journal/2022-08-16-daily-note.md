# Tuesday, August 16, 2022

[[Journal]]
[[javascript]]

## 콜백 함수란?

- 다른 함수의 인자로써 이용되는 함수.
- 어떤 이벤트에 의해 호출되어지는 함수.

### 다른 함수의 인자로써 이용되는 함수

```js
//hello!
function printHello() {
    print("hello");
}
//bye!
function printBye() {
    print("bye");
}

//특정 함수를 매개변수로 받아서 3초 뒤에 실행하는 함수
function sleepAndExecute(sleepTimeSecond, callback) {
    //sleepTimeSecond 초 만큼 대기
    sleep(sleepTimeSecond);
    //전달된 callback 실행
    callback();
}

//3초 뒤에 hello 출력하기
sleepAndExecute(3, printHello);

//5초 뒤에 bye 출력하기
sleepAndExecute(5, printBye);
```

### 어떤 이벤트에 의해 호출되어지는 함수

```js
function onCableConnected() {
    print("케이블이 연결되었습니다");
}

//케이블이 연결될 때 마다 전달된 onCableConnected가 호출된다고 가정
setOnCableConnected(onCableConnected);
```

참고 : [https://satisfactoryplace.tistory.com/18](https://satisfactoryplace.tistory.com/18)
