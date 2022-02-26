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
useEffect(수행하고자 하는 작업: 단 한 번만 실행됨, deps);

예제:
useEffect(() => {
    <'calling api function'>
}, []);
```

이처럼 `useEffect()`의 `deps`에 빈 배열을 넣어주면, 상태값이 변경되어도, 컴포넌트가 최초 로딩된 이후에는 변경되지 않는다. 위처럼 api를 호출할 때 사용하기 좋다. 이런... 이전에 만들었던 사이트에선, api가 반복 호출 되는 것을 방지하기 위해, axios에 캐시 확인 기능을 추가하여 나도 잘 모르는 땜빵을 했었는데, 이를 이용했으면 더 이상 그럴 필요가 없었을텐데... (하지만 그땐 이미 class 컴포넌트를 사용중이였고, 따라서 지금 함수형 컴포넌트로 변경하고자 하는 이유도 이러한 간편함 때문이다.)

잠깐... 그런데 `deps`가 뭐지?