# Priority Queue
Priority Queue는 높은 우선순위를 가진 원소는 낮은 우선순위를 가진 원소보다 먼저 처리되는 자료구조

## 문제 예시
주어진 N(2<= N <=100)개의 수를 작은 숫자가 높은 우선순위를 갖는 Priority Queue에 저장하고, 우선 순위가 높은 숫자부터 차례대로 출력하시오. (입력에는 오류가 없다고 가정)

|Input|Output|
|---|---|
2 //테스트 케이스 수
10 //입력 수
|10 49 38 17 56 92 8 1 13 55 //입력 데이터|1 8 10 13 17 38 49 55 56 92
13
4 22 50 13 5 1 22 35 21 7 99 100 14|1 4 5 7 13 14 21 22 22 35 50 99 100

```python
MAX_SIZE = 100
heapSize = 0
heap = list()


def heapInit():
    global heapSize, heap
    heap.clear()
    for i in range(MAX_SIZE):
        heap.append(0)


def heapPush(value):
    global heapSize, heap
    if heapSize + 1 > MAX_SIZE:
        return
    heap[heapSize] = value

    current = heapSize
    while current > 0 and heap[current] < heap[(current - 1) // 2]:
        heap[(current - 1) // 2], heap[current] = heap[current], heap[(current - 1) // 2]
        current = (current - 1) // 2

    heapSize = heapSize + 1


def heapPop():
    global heapSize, heap

    if heapSize <= 0:
        return -1

    value = heap[0]
    heapSize = heapSize - 1

    heap[0] = heap[heapSize]

    current = 0
    while current < heapSize and current * 2 + 1 < heapSize:
        child = 0
        if current * 2 + 2 >= heapSize:
            child = current * 2 + 1
        else:
            if heap[current * 2 + 1] < heap[current * 2 + 2]:
                child = current * 2 + 1
            else:
                child = current * 2 + 2

        if heap[current] < heap[child]:
            break

        heap[current], heap[child] = heap[child], heap[current]
        current = child

    return value


def main():
    T = int(input())

    for test_case in range(1, T + 1):
        N = int(input())
        heapInit()

        value = list(map(int, input().split()))

        for i in range(N):
            heapPush(value[i])

        print("#%d" % test_case, end=' ')
        for i in range(N):
            print(heapPop(), end=' ')
        print()
```

```java
import java.util.Scanner;

class Solution {
	static Scanner sc;

	static final int MAX_SIZE = 100;

	static int heap[] = new int[MAX_SIZE];
	static int heapSize = 0;

	static void heapInit()
	{
		heapSize = 0;
	}

	static void heapPush(int value)
	{
		if (heapSize + 1 > MAX_SIZE)
		{
			return;
		}

		heap[heapSize] = value;

		int current = heapSize;
		while (current > 0 && heap[current] < heap[(current - 1) / 2]) 
		{
			int temp = heap[(current - 1) / 2];
			heap[(current - 1) / 2] = heap[current];
			heap[current] = temp;
			current = (current - 1) / 2;
		}

		heapSize = heapSize + 1;
	}

	static int heapPop()
	{
		if (heapSize <= 0)
		{
			return -1;
		}

		int value = heap[0];
		heapSize = heapSize - 1;

		heap[0] = heap[heapSize];

		int current = 0;
		while (current < heapSize && current * 2 + 1 < heapSize)
		{
			int child;
			if (current * 2 + 2 >= heapSize)
			{
				child = current * 2 + 1;
			}
			else
			{
				child = heap[current * 2 + 1] < heap[current * 2 + 2] ? current * 2 + 1 : current * 2 + 2;
			}

			if (heap[current] < heap[child])
			{
				break;
			}

			int temp = heap[current];
			heap[current] = heap[child];
			heap[child] = temp;

			current = child;
		}
		return value;
	}

	static void heapPrint(int[] heap, int heap_size)
	{
		for (int i = 0; i < heap_size; i++)
		{
			System.out.print(heap[i] + " ");
		}
		System.out.println();
	}


	public static void main(String arg[]) throws Exception {
		sc = new Scanner(System.in);
		
		int T = sc.nextInt();

		for (int test_case = 1; test_case <= T; test_case++)
		{
			int N = sc.nextInt();
			
			heapInit();
			
			for (int i = 0; i < N; i++)
			{
				int value = sc.nextInt();
				heapPush(value);
			}

			System.out.print("#" + test_case + " ");			
			for (int i = 0; i < N; i++)
			{
				System.out.print(heapPop() + " ");
			}
			System.out.println();
		}
		sc.close();
	}
}```