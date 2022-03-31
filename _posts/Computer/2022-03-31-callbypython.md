---
title: "2022-03-31-callbypython"
lang: "ko"
layout: page
date: 2022-03-31 23:01:34 +9000
tags: "Computer"
type: documents
---
<!-- [[Computer]] -->

## call by reference, call by value

C언어를 공부하다 Call by reference와 Call by value에 대해 살펴보게 되었다. 가령, 다음과 같은 프로그램에서 선언된 두 변수의 값은 서로 뒤바뀐다:

```c
#include <stdio.h>

void swap(int *num1, int *num2) {
    int temp = *num1;
    *num1 = *num2;
    *num2 = temp;
}

int main() {
    int num1 = 10;
    int num2 = 20;

    printf("num1의 값 : %d\n", num1);
    printf("num2의 값 : %d\n", num2);

    swap(&num1, &num2);
    printf("\nSwap함수 실행 후\n\n");
    
    printf("num1의 값 : %d\n", num1);
    printf("num2의 값 : %d\n", num2);

    return 0;
}
```

두 변수를 서로 뒤바꾸는 swap 함수는 두 변수의 주소를 인자로 받는데, 이를 call by reference라고 한다. 인자가 변수의 주소를 가리키는 포인터 변수이기 때문에, 해당 인자를 조작하면 원본 변수의 주소의 값과 연결되어, 원본 변수의 값을 바꿔버린다. 만약 주소가 아닌 변수의 값을 함수에 넘기고, 함수는 변수의 값을 인자로 받는 경우, 이를 call by value라고 한다. 함수에 매개변수로 원본 변수의 값을 전달하였을 때에는, (주소가 아닌) 값이 복사되어 함수로 전달되기 때문에, 해당 함수 내에서만 처리될 뿐, 원본 변수의 값 자체는 변하지 않는다.

그렇다면 Python의 경우에는 call by reference일까, call by value일까? 재미있게도, Python은 **passed by assignment**의 방식으로 함수의 인자를 전달한다고 한다.

## passed by assignment

passed by assignment란, 쉽게 말한다면, 할당하는 값에 따라 call by reference와 call by value가 결정되는 방식이라고 말할 수 있을 것 같다.

이것이 가능한 이유는, Python은 *모든 것을 "객체"로 판단*하기 때문이다. 즉, Python의 변수는 C언어의 변수처럼 특정한 메모리 공간을 할당받은 컨테이너가 아닌, *어떤 객체에 붙여진 이름표*와 같은 것이다. 가령 `a = 1`이라는 변수를 선언하는 경우, `1`이라는 객체가 생성되고, 이어서 `a`라는 변수가 해당 객체를 가리키도록 한다. 따라서 객체의 특성에 따라 변수가 작동하는 방식이 결정된다.

Python의 자료형은 크게 불변(immutable)과 가변(mutable)이 있다. int, str, bool, tuple같은 타입은 불변 자료형이고, list, dictionary 같은 타입은 가변 자료형이다. 그리고 불변 타입의 객체를 넘기면 call by value가 되고, 가변 타입의 객체를 넘기면 call by reference가 된다.

불변 타입의 객체가 함수의 인자로 넘어가게 되면, 해당 객체는 함수 안에서 새로운 객체로 생성된다. 반면 가변 타입의 객체는 새로 생성되지 않고, 기존의 객체에서 조작이 발생하게 된다. ('불변'과 '가변'의 의미를 다시 한 번 눈여겨보자!) 다음의 Python 프로그램에서 이를 명확하게 확인할 수 있다:

```python
def change(b, s, n, l, d):
    b = True
    s = '새로운 문자'
    n = n + 100
    l.append(100)
    d['과학'] = 70

b = False           # bool
s = '기존 문자'     # string
n = 10              # int
l = [1,2,3]         # list
d = {'국어': 80}    # dictionary
change(b, s, n, l, d)

print(b)
print(s)
print(n)
print(l)
print(d)

"""
output:
False
기존 문자
10
[1,2,3,100]
{'국어': 80, '과학': 70}
"""
```

따라서 가변 객체의 경우, 기존 객체가 함수에 의해 함부로 변경되지 않도록 주의를 기울여야 한다. 만약 해당 list 객체의 값을 다루고 싶지만, 기존 객체가 변경되는 것은 원하지 않는다면, 아래와 같이 list를 복사하거나, `copy()` 메소드를 사용해야 한다.

```python
a = [10, 20]
b = a[:]
c = a.copy()

b.append(30)
c.pop()
print(a,b,c)

"""
output:
[10, 20], [10, 20, 30], [10]
"""
```

참고 블로그
- https://foramonth.tistory.com/20
- https://coding-factory.tistory.com/636
- https://alcohol98holic.tistory.com/20
- https://aalphaca.tistory.com/4
- https://eslife.tistory.com/1053
