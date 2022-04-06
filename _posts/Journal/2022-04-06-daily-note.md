# Wednesday, April 6, 2022

[[Journal]]

버블정렬은 가장 큰 값이 맨 뒤로 질질 끌려가는 과정이다. 그렇기에 매 턴마다 가장 큰 값은 제외해도 된다.

```c
#include <stdio.h>

void bubble_sort(int arr[], int count)
{
    int temp;

    for (int i = 0; i < count; i++) {
        for (int j = 0; j < count -i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

int main()
{
    int numArr[10] = { 8, 4, 2, 5, 3, 7, 10, 1, 6, 9 };

    bubble_sort(numArr, sizeof(numArr) / sizeof(int));

    for (int i = 0; i < sizeof(numArr) / sizeof(int); i++) {
        printf("%d ", numArr[i]);
    }
    
    printf("\n");

    return 0;
}
```

함수형은 함수의 반환값과 일치해야 한다(return값이 정수면, 함수도 정수형)
void 포인터는 참조할 수 없으므로, 먼저 포인터 형변환을 한 뒤, 역참조를 한다`(*(int *)num1)`
