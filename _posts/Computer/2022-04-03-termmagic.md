---
title: "2022-04-03-termmagic"
lang: "ko"
layout: page
date: 2022-04-03 20:02:57 +9000
tags: "Computer"
type: documents
---
<!-- [[Computer]] -->
Terminal Magic... 터미널에 무한으로 무지개 그라데이션 만들기!

```zsh
yes "$(seq 231 -1 16)" | while read i; do printf "\x1b[48;5;${i}m\n"; sleep .02; done
```

명령어를 하나씩 살펴보자.

- `yes` : 반복해서 명령어를 출력한다. 영원히!

```zsh
yes 'All Work and No Play makes Jack a dull boy'
```

- `seq` : 일련의 연속된 숫자를 출력한다.
  - 1부터 3까지, 1씩 증가되는 숫자의 연속
  
  ```zsh
  > seq 1 3
  1
  2
  3
  ```

  - 따라서, 위 명령어의 `"$(seq 231 -1 16)"`는, 231부터 16까지 1씩 감소하는 숫자의 연속을 출력한다.

- `while`... : `while` 반복문 안에 담긴 명령어를 수행한다.
  - 이 경우, 중단 조건이 없으니(`yes`로 무한한 수의 출력이 주어지기에), 무한히 `printf`의 명령어를 수행하게 된다. 그런데 `printf`에 작성된 저 외계어는 무엇일까?
  - 그것은 바로... **ANSI Color Codes**이다. ANSI 코드를 사용하여 프롬프트 텍스트의 글자의 색을 바꿀 수 있다.
  - `\x1b[48;5;[n]m\n`은 배경색을 출력하는 코드이다. n의 범위는 0부터 255까지이며, 0부터 15는 기본 컬러 테이블, 232부터 255는 그레이스케일 컬러 테이블, 중간의 숫자들은 256 컬러 테이블이다. 참고로 `\x1b[38;5;[n]m\n`은 글자색을 출력하는 코드로, 글자에 특정한 색을 입히고자 한다면, `printf "\x1b[38;5;53mHello\n"`와 같이 입력하면 된다.
  - 따라서 `while`문은 `yes` 명령어로부터 무한히 주어지는 16~231을 인덱스로 하여 배경색을 0.2초 간격으로 무한히 출력한다.

![termmagic](/attachments/termmagic.png)

![heman](/attachments/2022-04-03-20-28-21.png)

참고자료:

[기계인간님 블로그](https://johngrib.github.io/wiki/yes-cmd/)

[ANSI Color Codes](https://talyian.github.io/ansicolors/)
