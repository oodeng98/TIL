# Deque(덱)

Deque은 Queue의 일반화 된 버전으로 Queue의 양 끝에서 데이터의 삽입/삭제가 가능하게끔 만든 자료구조

## 문제 예시

크기가 N 인 Queue 에서 주어진 M 개의 Command 를 수행하는 프로그램을 작성하라.  
각각의 Command는 1~6 사이의 정수값을 가지며, 다음과 같은 동작을 수행한다.

1: front에 element 삽입  
2: rear에 element 삽입  
3: front에 있는 element 출력  
4: rear에 있는 element 출력  
5: front에 있는 element 삭제  
6: rear에 있는 element 삭제

## 구현

```python
from collections import deque

lst = deque([])  # 이제 lst를 deque으로 활용할 수 있음
```

```java
import java.util.Scanner;

class Solution {

	static final int MAX = 100;

	static int arr[];
	static int front;
	static int rear;
	static int size;

	static void dequeInit(int size) {
		arr = new int[MAX];
		front = -1;
		rear = 0;
		Solution.size = size;
	}

	static boolean isFull() {
		return ((front == 0 && rear == size - 1) || front == rear + 1);
	}

	static boolean isEmpty() {
		return (front == -1);
	}

	static void insertFront(int value) {
		if (isFull()) {
			System.out.println("Overflow");
			return;
		}

		if (front == -1) {
			front = 0;
			rear = 0;
		} else if (front == 0) {
			front = size - 1;
		} else {
			front = front - 1;
		}
		arr[front] = value;
	}

	static void insertRear(int value) {
		if (isFull()) {
			System.out.println("Overflow");
			return;
		}

		if (front == -1) {
			front = 0;
			rear = 0;
		} else if (rear == size - 1) {
			rear = 0;
		} else {
			rear = rear + 1;
		}
		arr[rear] = value;
	}

	static int getFront() {
		if (isEmpty()) {
			System.out.println("Underflow");
			return -1;
		}
		return arr[front];
	}

	static int getRear() {
		if (isEmpty() || rear < 0) {
			System.out.println("Underflow");
			return -1;
		}
		return arr[rear];
	}

	static void deleteFront() {
		if (isEmpty()) {
			System.out.println("Underflow");
			return;
		}

		if (front == rear) {
			front = -1;
			rear = -1;
		} else if (front == size - 1) {
			front = 0;
		} else {
			front = front + 1;
		}
	}

	static void deleteRear() {
		if (isEmpty()) {
			System.out.println("Underflow");
			return;
		}

		if (front == rear) {
			front = -1;
			rear = -1;
		} else if (rear == 0) {
			rear = size - 1;
		} else {
			rear = rear - 1;
		}
	}

	public static void main(String arg[]) throws Exception {
		Scanner sc = new Scanner(System.in);

		int T = sc.nextInt();

		for (int test_case = 1; test_case <= T; test_case++) {
			int N = sc.nextInt();
			int M = sc.nextInt();

			dequeInit(N);

			System.out.print("#" + test_case + " ");

			for (int i = 0; i < M; i++) {
				int cmd = sc.nextInt();

				switch (cmd) {
				case 1: {
					int elem = sc.nextInt();
					insertFront(elem);
					break;
				}
				case 2: {
					int elem = sc.nextInt();
					insertRear(elem);
					break;
				}
				case 3: {
					System.out.print(getFront() + " ");
					break;
				}
				case 4: {
					System.out.print(getRear() + " ");
					break;
				}
				case 5: {
					deleteFront();
					break;
				}
				case 6: {
					deleteRear();
					break;
				}
				}
			}

			System.out.println();

		}
		sc.close();
	}
}
```
