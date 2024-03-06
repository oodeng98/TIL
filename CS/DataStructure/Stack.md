# Stack
Stack은 한 쪽 끝에서만 자료를 넣거나 뺄 수 있는 선형 구조(LIFO - Last In First Out)로 데이터를 저장하는 형식

## 문제 예시
주어진 N(2<= N <=100)개의 수를 순서대로 Stack에 넣은 후 하나씩 꺼내 화면에 출력하시오.
|Input|Output|
|---|---|
2 // 테스트 케이스 수
5 // 데이터 크기
1 2 3 4 5|5 4 3 2 1
5
5 4 2 3 1|1 3 2 4 5

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

```java
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