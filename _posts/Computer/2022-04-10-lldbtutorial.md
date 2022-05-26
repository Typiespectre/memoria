---
title: "2022-04-10-lldbtutorial"
lang: "ko"
layout: page
date: 2022-04-10 19:30:47 +9000
tags: "Computer"
type: documents
---
<!-- [[Computer]] -->

예전부터 디버거를 사용하는 멋진 프로그래밍을 하겠다는 마음을 잡았었지만, 고질적인 게으름때문인지 언제나 print문을 덧붙여 무식하게 무한정 프로그램을 돌려보는 야수의 행동방식(...)을 취하였다. 마침 내일 서울42 라피신이 시작되고, C언어를 쑤셔먹게되는 김에, 디버거에 익숙해지고자 **lldb(Low-Level Debugger)** 사용법 및 튜토리얼을 정리해보고자 한다.

LLDB는 Xcode의 기본 디버거로 내장되어있기에, 맥(Mac)을 사용한다면 터미널에서 바로 사용할 수 있다. 만약 유명한 리눅스 디버거인 GDB를 사용하고자 한다면, 맥에 인증서도 설정해야 하고 생각보다 귀찮은 일들을 해야했던 기억이 난다. 음... 그리고 내 기억으론, 맥이 GCC 컴파일러에서 LLVM/Clang 컴파일러로 갈아타게 되었기에(라이선스가 달라서 그런가? GCC는 GPL 라이선스이고, LLVM은 BSD 라이선스라고 한다. BSD 라이선스는 GPL 라이선스와 달리, 해당 라이선스를 이용한 프로그램을 배포할 때, 프로그램의 소스 코드를 공개/배포할 필요가 없다.), C언어를 빌드할 때 터미널에서 여전히 `gcc` 명령어를 사용할 수 있는걸로 알고 있지만, 실제 컴파일은 `cc`(LLVM/Clang 컴파일러)로 진행된다는 이야기를 들었던 것 같다.

```zsh
> gcc --version
Apple clang version 13.1.6 (clang-1316.0.21.2)
Target: x86_64-apple-darwin21.4.0
Thread model: posix
InstalledDir: /Library/Developer/CommandLineTools/usr/bin

> cc --version
Apple clang version 13.1.6 (clang-1316.0.21.2)
Target: x86_64-apple-darwin21.4.0
Thread model: posix
InstalledDir: /Library/Developer/CommandLineTools/usr/bin

# NANI??!!
```

- LLVM? Clang?
    간단하게 설명하면, LLVM은 컴파일러 구조라고 말할 수 있을 것 같다. 컴파일러를 프론트엔드와 백엔드로 구분한다면, Clang이라는 컴파일러가 프론트엔드의 위치에서 프로그램의 소스 코드를 LLVM이 이해할 수 있게 컴파일하고, LLVM 내부의 LLVM Core가 백엔드의 위치에서 컴파일된 코드를 최적화하고 기계어로 번역한다. 보다 자세한 LLVM의 이야기는 [LLVM 나무위키](https://namu.wiki/w/LLVM)를 참고하자.

아무튼, LLDB의 사용법은 GDB와 굉장히 유사하기에, GDB에 익숙한 사람들도 쉽게 사용할 수 있다고 한다. 또한 지금처럼 간단한 C파일은 무한 print문 + 빌드로 진행과정을 살펴볼 수 있겠지만, 만약 거대한 프로젝트라면 빌드시간이 어마무시하게 길어지게 될 것이고 문제의 복잡도도 그만큼 커지기 때문에, 전체 프로그램을 한 번에 살펴볼 수 있는 디버거가 유용해지게 될 것이다.

## LLDB 사용법

우선 디버깅을 하기 위해선 컴파일 옵션에 `-g` 플래그를 설정해서 실행 파일을 빌드해야 한다.

```zsh
# cc를 사용해도 문제없다.
> gcc -g main.c -o app.out
```

그리고 LLDB를 사용하기 위해선, 아래와 같이 명령어를 입력하자.

```zsh
> lldb app.out
(lldb) target create "app.out"
Current executable set to 'app.out' (x86_64).
(lldb)
```

LLDB를 실행하면 터미널 입력 커서가 `(LLDB)`로 변경되고, 입력을 기다리는 상태가 된다. 프로그램을 실행하기 위해선 `run` 커맨드를 입력하면 된다. 만약 프로그램 실행 중 `run` 명령어를 입력하면, 현재 프로세스를 끝내고 다시 새로 빌드하여 프로세스를 실행한다.

```zsh
(lldb) run

# 혹은
(lldb) r

# 만약 argument를 넘겨야 하는 프로그램이라면
(lldb) run "arg1"
# 이렇게 하면 파일의 argv[1]에 "arg1"이 들어간다.
```

만약 문제가 있는 프로그램이라면 아래처럼 오류메세지를 띄우고 문제가 있는 부분을 자동으로 지시한다. (아래 예시는 무한 재귀 호출로 인한 segmentation fault 오류가 발생하는 피보나치 코드를 디버깅한 경우이다.)

```zsh
(lldb) run
Process 19865 launched: '/Users/memoria/app.out' (x86_64)
/*** fib ***/
Process 19865 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = EXC_BAD_ACCESS (code=2, address=0x3038cbff8)
    frame #0: 0x0000000100003a28 app.out`fib(n=<unavailable>) at main.c:16
   13  
   14  
   15  unsigned int fib(unsigned int n)
-> 16  {
   17    if (n==0)
   18      return 0;
   19    else if (n==1)
Target 0: (app.out) stopped.
```

LLDB가 전체 코드를 보여주지 않기에, 만약 해당 코드의 일부분을 좀 더 자세하게 보고싶다면 `list` 명령어를 입력하면 된다.

```zsh
(lldb) list 16
16  {
17    if (n==0)
18      return 0;
19    else if (n==1)
20      return 1;
21    else
22      return fib(n-1) + fib(n);
23  }
24
25
```

위처럼 문제가 있는 부분을 발견하였다면 해당 라인이나 살펴보고 싶은 함수에 breakpoint를 걸면 된다. 문제가 없는 함수라도 breakpoint를 걸지 않으면 그냥 정상종료가 되어버리기 때문에, 동일하게 라인 또는 함수에 breakpoint를 걸어 해당 프로그램이 실행되다가 설정한 breakpoint에 멈추도록 한다.

```zsh
# breakpoint 함수에 걸기
(lldb) b fib
Breakpoint 1: where = app.out fib + 11 at main.c:17:8, address = 0x0000000100003a2b

# breakpoint 라인에 걸기
(lldb) b 16
Breakpoint 2: where = app.out fib + 11 at main.c:17:8, address = 0x0000000100003a2b

# breakpoint 제거
(lldb) br del 2
1 breakpoints deleted; 0 breakpoint locations disabled.

# breakpoint 리스트 보기
(lldb) br l
Current breakpoints:
1: name = 'fib', locations = 1, resolved = 1, hit count = 2
  1.1: where = app.out fib + 11 at main.c:17:8, address = 0x0000000100003a2b, resolved, hit count = 2

# 모든 breakpoint 제거
(lldb) br del
About to delete all breakpoints, do you want to do that?: [Y/n] Y
All breakpoints removed. (1 breakpoint)
```

breakpoint를 설정하였다면 코드를 실행하자. `next`, `step` 명령어는 코드를 한 줄씩 실행하고, `continue`는 코드를 다음 breakpoint까지 실행한다. `next`와 `step` 명령어의 차이는, `next`는 코드 실행 중 함수가 있어도 함수 안으로 들어가지 않지만, `step`은 코드 실행 중 함수가 있다면 함수 안으로 들어간다. 함수를 빠져나오고자 한다면 `finish` 명령어를 입력하면 된다.

- `next`는 `n`, `step`은 `s`, `continue`는 `c`로 줄여서 입력할 수 있다.
- 사실 `finish`는 함수를 빠져나온다기 보단, 현재 프레임을 빠져나온다고 말할 수 있다. 기본적으로 프로그램이 실행되면 일반적인 프로그램 아래에 어셈블리 프레임이 깔리게 되는 것 같은데, 그렇기에 `finish`를 입력해도 어셈블리 창이 뜨는 경우가 발생할 수도 있다. 당황하지 말고 `continue`하여 프로그램을 종료하자.

이제 우리가 디버거에서 원하는 기능, 즉 변수의 상태를 확인하기 위해선, `fr v`(frame variable) 명령어를 입력하면 된다. 혹은 `print` 명령어로 함수의 변수를 살펴볼 수도 있다. (`print`는 `p`로 줄여쓸 수 있다.)

```zsh
# 전체 변수값 출력
(lldb) fr v
(unsigned int) n = 2

(lldb) p n
(unsigned int) $1 = 2

# print 명령어로 변수의 주소도 구할 수 있다.
(lldb) p &n
(unsigned int *) $2 = 0x00000003040c7554
```

그런데 만약 위와 같은 재귀함수라면, 계속해서 명령어를 입력해야 하고, 변수의 값만으로 상태를 확인하는데 불편함이 있을 수 있다. 그럴 땐 `bt`(stack backtrace) 명령어를 이용하면, 현재 스레드가 어떠한 방식으로 진행되는지 확인할 수 있다.

```zsh
# 만약 수십 번 재귀함수를 호출하였다면, 다음과 같은 스택이 쌓이게 될 것이다.
(lldb) bt
* thread #1, queue = 'com.apple.main-thread', stop reason = step out
Return value: (unsigned int) $9 = 1

  * frame #0: 0x0000000100003a62 app.out`fib(n=2) at main.c:22:12
    frame #1: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #2: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #3: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #4: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #5: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #6: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #7: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #8: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #9: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #10: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #11: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #12: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #13: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #14: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #15: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #16: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #17: 0x0000000100003a6d app.out`fib(n=2) at main.c:22:23
    frame #18: 0x0000000100003be5 app.out`main(argc=1, argv=0x00000003040c76a0) at main.c:85:21
    frame #19: 0x000000020001551e dyld`start + 462

# 함수가 fib(n=2)에서 전혀 변하고 있지 않기에, 무한루프에 빠져있음을 확인할 수 있다.
```

진행과정을 쭉 정리해보면:

1. 먼저 `-g` 옵션으로 소스코드를 컴파일한다.
2. LLDB를 실행파일과 함께 실행한다.
3. 살펴보고 싶은 지점에 breakpoint를 건다.
4. `run`으로 실행한 후, 한 단계씩 나아가면서, `fr v`와 같은 명령어로 변수와 함수의 상태를 확인해본다.

LLDB를 종료하기 위해선 `quit` 명령어를 입력하자. 만약 무언가 잘못 입력해서 LLDB가 완전히 뻗어버린다면, CTRL+Z를 눌러 현재 프로세스를 백그라운드로 보내버리고(왜냐하면 내 경험상 CTRL+C도 작동하지 않았기에...) 터미널에 `kill -9 프로세스ID`를 입력하여 중지하자. 음... 별로 좋은 방법은 아닌 것 같다는 직감이 강하게 들지만...

만약 도움말이 필요하다면, LLDB 실행창 안에서 `help <명령어>`를 입력하면 된다.

지금까지가 기본적인 LLDB 설명이였다면, LLDB로 다양한 활동을 할 수도 있다. 가령, `p &(변수)`로 변수의 주소를 구하였다면, `memory read`로 해당 변수의 메모리를 탐색할 수도 있다.

```zsh
(lldb) p &n
(int *) $3 = 0x000000010869f510

# memory read를 아래처럼 줄일 수도 있다.
(lldb) m read $3
0x10869f510: 02 00 00 00 03 00 00 00 00 00 00 00 00 00 00 00  ................
0x10869f520: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
```

LLDB의 보다 응용적인 사용법은 이후에 LLDB를 사용하면서 발견하게 되면 하나씩 추가하겠다. 내일 라피신을 시작하는데, 긴장을 풀기 위해 글을 끄적거리다보니 어느새 12시가 다 되어가는군... 내일부터 지치지 말고 잘 달려서 완주하고, 합격하자!

-------

참고자료:

[https://lldb.llvm.org/use/map.html](https://lldb.llvm.org/use/map.html)

[https://www.classes.cs.uchicago.edu/archive/2017/winter/15200-2/assigns/week5/lldb.html](https://www.classes.cs.uchicago.edu/archive/2017/winter/15200-2/assigns/week5/lldb.html)

[https://hyeyoo.com/64](https://hyeyoo.com/64)

[https://green1229.tistory.com/83](https://green1229.tistory.com/83)

앞으로 참고하면 좋을 자료:

[https://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-and-lldb](https://minsone.github.io/ios/mac/xcode-lldb-debugging-with-xcode-and-lldb)

[https://kapeli.com/cheat_sheets/LLDB_Commands.docset/Contents/Resources/Documents/index](https://kapeli.com/cheat_sheets/LLDB_Commands.docset/Contents/Resources/Documents/index)

[https://aaronbloomfield.github.io/pdr/docs/lldb_summary.html](https://aaronbloomfield.github.io/pdr/docs/lldb_summary.html)

[Back](/)
