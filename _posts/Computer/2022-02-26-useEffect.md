---
title: "2022-02-26-useEffect"
lang: "ko"
layout: page
date: 2022-02-26 21:39:33 +9000
tags: "Computer"
type: documents
---
<!--  [[Computer]] -->

리액트에서 `useState()`를 이용하여 상태값(state)을 변경하면 모든 컴포넌트를 다시 로딩해야 한다. 이러한 반복적인 연산을 줄일 수 있는 방법이 없을까? `useEffect()`를 사용하면 해결된다.

`useeffect()`는 다음의 형태를 가진다:

```javascript
useEffect(수행하고자 하는 작업, deps);

예제:
useEffect(() => {
    <'Calling Api Function'>
}, []);
```

이처럼 `useEffect()`의 `deps`에 빈 배열을 넣어주면, 상태값이 변경되어도, 컴포넌트가 최초 로딩된 이후에는 변경되지 않는다. 위처럼 api를 호출할 때 사용하기 좋다. 이런... 이전에 만들었던 사이트에선, api가 반복 호출 되는 것을 방지하기 위해, axios에 캐시 확인 기능을 추가하여 나도 잘 모르는 땜빵을 했었는데, 이를 이용했으면 더 이상 그럴 필요가 없었을텐데... (하지만 그땐 이미 class 컴포넌트를 사용중이였고, 따라서 지금 함수형 컴포넌트로 변경하고자 하는 이유도 이러한 간편함 때문이다.)

잠깐... 그런데 `deps`가 뭐지? 그리고 `deps`에 빈 배열을 넣으면 왜 한 번만 실행되는 것일까? `deps`는 특정 값을 업데이트 할 때, `deps`에 설정된 값을 기준으로 한다. 그렇기에 빈 배열이 있다면, `deps`가 참고할 값이 없으니 한 번만 수행되고 말지만, 만약 `deps`에 설정된 값이 있다면, 해당 값을 기준으로 하여 특정 값을 업데이트 한다. 예를 들어,

```javascript
useEffect(() => {
    <'Searching Keyword Function'>
}, [keyword]);
```

`deps`에 keyword 요소를 추가하면, keyword 요소에 변화가 생길 때마다, `useEffect()`가 실행된다. 정말로 강력하군!

재미있게도, 컴포넌트를 삭제할 때 `useEffect()`를 이용하여 특정한 함수를 실행할 수도 있다. 이를 Cleanup function이라고 부른다. 가령,

```javascript
function Compoenent() {
    useEffect(() => {
        <'Creating Function'>;
        return () => <'Destroying Function'>;
    }, []);
    return <h1>Compoenent</h1>;
}
```

해당 컴포넌트를 실행하면, 특정한 생성 함수를 단 한 번 실행한다. 그리고 해당 컴포넌트를 제거한다면, 삭제 함수를 실행한다. 이는 사실 단순한데, 생성 함수(위의 Creating Function)의 리턴값을 삭제 함수로 설정한 것이여서, 해당 함수가 끝나면(즉 제거되면) 자동으로 리턴값을 반환하여, 삭제 함수가 실행되게 하는 것이다. 역시 유용한 기능이다!

[Back](/)
