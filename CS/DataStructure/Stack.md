# Stack

물건을 쌓아 올리듯 자료를 쌓아 올린 형태의 자료구조

- 스택에 저장된 자료는 선형 구조를 가짐
- 마지막에 삽입한 자료를 가장 먼저 꺼냄(LIFO: Last In First Out)

## 간단한 구현

자료를 선형으로 저장할 저장소 필요(배열 사용 가능)  
top: 스택에 마지막으로 삽입된 원소의 위치

- push(삽입): 저장소에 자료를 저장, push라고 함

```python
def push(item, size):
    global top
    top += 1
    if top == size:
        print('overflow')
    else:
        stack[top] = item

size = 10  # size를 정하지 않고 동적으로 할당하는 방법을 사용하는 경우가 더 빈번
stack = [0] * size
top = -1
```

- pop(삭제): 저장소에서 자료를 꺼냄

```python
def pop():
    global top
    if top == -1:
        print('underflow')
        return 0
    else:
        top -= 1
        return stack[top + 1]
```

- isEmpty: 스택이 공백인지 아닌지 확인
- peek: 스택의 top에 있는 item(원소)을 반환

## 문제 예시

주어진 N(2<= N <=100)개의 수를 순서대로 Stack에 넣은 후 하나씩 꺼내 화면에 출력하시오.
|Input|Output|
|---|---|
2 // 테스트 케이스 수
5 // 데이터 크기
1 2 3 4 5|5 4 3 2 1
5
5 4 2 3 1|1 3 2 4 5

## 구현

```python
stack = []

def stackInit():
    global stack
    stack.clear()


def stackIsEmpty():
    global stack
    return len(stack) == 0


def stackPush(value):
    stack.append(value)


def stackPop():
    return stack.pop()


def main():
    T = int(input())

    for test_case in range(1, T + 1):
        N = int(input())

        stackInit()

        l = list(input().split())

        for i in l:
            stackPush(i)

        print("#%d" % test_case, end=' ')

        while not stackIsEmpty():
            value = stackPop()
            if value != None:
                print(value, end=' ')

        print()
```

````java
import java.util.Scanner;

class Solution {

	static final int MAX_N = 100;

	static int top;
	static int stack[] = new int[MAX_N];

	static void stackInit()
	{
		top = 0;
	}

	static boolean stackIsEmpty()
	{
		return (top == 0);
	}

	static boolean stackIsFull()
	{
		return (top == MAX_N);
	}

	static boolean stackPush(int value)
	{
		if (stackIsFull())
		{
			System.out.println("stack overflow!");
			return false;
		}
		stack[top] = value;
		top++;

		return true;
	}

	static Integer stackPop()
	{
		if (top == 0)
		{
			System.out.println("stack is empty!");
			return null;
		}

		top--;
		Integer value = new Integer(stack[top]);

		return value;
	}

	public static void main(String arg[]) throws Exception {
		Scanner sc = new Scanner(System.in);

		int T = sc.nextInt();

		for (int test_case = 1; test_case <= T; test_case++)
		{
			int N = sc.nextInt();

			stackInit();
			for (int i = 0; i < N; i++)
			{
				int value = sc.nextInt();
				stackPush(value);
			}

			System.out.print("#" + test_case + " ");

			while (!stackIsEmpty())
			{
				Integer value = stackPop();
				if (value != null)
				{
					System.out.print(value.intValue() + " ");
				}
			}
			System.out.println();
		}
		sc.close();
	}
}```
````
