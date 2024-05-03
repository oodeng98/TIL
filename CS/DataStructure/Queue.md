# Queue

스택과 마찬가지로 삽입과 삭제의 위치가 제한적인 자료구조

- 큐의 뒤에서는 삽입만 하고, 큐의 앞에서는 삭제만 이루어지는 구조
- 먼저 삽입된 원소는 먼저 삭제됨(FIFO: First In First Out)

## 간단한 Queue 구현

자료를 선형으로 저장할 저장소 필요
front: 최근 삭제된 위치
rear: 저장된 원소 중 마지막 원소

- enQueue: 큐의 rear 다음에 원소를 삽입

```python
def enQueue(item):
    global rear
    if isFull():
        print("Queue is Full")
    else:
        rear += 1
        Queue[rear] = item
```

- deQueue: 큐의 front에서 원소를 삭제하고 반환

```python
def deQueue():
    if isEmpty():
        print("Queue is Empty")
    else:
        front += 1
        return Queue[front]
# 실제로 삭제하는 것은 아니고 새로운 원소를 리턴하면서 삭제와 동일한 기능을 함
```

- createQueue: 비어있는 큐를 생성  
  초기 상태: front = rear = -1
- isEmpty: 큐가 비어있는지 확인
  공백 상태: front == rear

```python
def isEmpty():
    return front == rear
```

- isFull: 큐가 가득 찼는지 확인
  포화 상태: rear == n - 1(n: 배열의 크기)

```python
def isFull():
    return rear == len(Queue) - 1
```

- Qpeek(): 큐의 front에서 원소를 삭제 없이 반환

```python
def Qpeek():
    if isEmpty():
        print("Queue is Empty")
    else: return Q[front+1]
```

## Circular Queue

선형 큐 이용시 원소의 삽입과 삭제를 계속할 경우, 배열의 앞부분에 활용할 수 있는 공간이 있음에도 불구하고 rear == n-1인 포화상태로 인식하여 더 이상의 삽입이 불가능해짐  
간단한 해결방법으로 매 연산이 이루어질 때마다 저장된 원소들을 배열의 앞부분으로 모두 이동시키는 방법이 있지만, 원소 이동에 많은 시간이 소요되어 효율성이 급격히 떨어진다.

- 그래서 1차원 배열을 사용하되, 논리적으로 배열의 처음과 끝이 연결된 원형 형태의 큐를 이룬다고 가정하고 사용

### index의 순환

front와 rear의 위치가 배열의 마지막 인덱스인 n-1을 가리킨 후, 배열의 처음 인덱스인 0으로 이동해야 함  
이를 위해 나머지 연산자 mod 활용

### 간단한 Circular Queue 구현

- isEmpty: 큐가 비어있는지 확인
  공백 상태: front == rear

```python
def isEmpty():
    return front == rear
```

- isFull: 큐가 가득 찼는지 확인
  포화 상태: (rear + 1) mod n == front(n: 배열의 크기)

```python
def isFull():
    return (rear + 1) % len(Queue) == front
```

[왜 원형 큐는 한자리를 사용하지 않는가](./Circular_Queue.md)

- enQueue: 원형 큐이므로 rear = (rear + 1) mod n을 활용

```python
def enQueue(item):
    global rear
    if isFull():
        print("Queue is Full")
    else:
        rear = (rear + 1) % len(Queue)
        Queue[rear] = item
```

- deQueue: 마찬가지로 원형 큐이므로 front = (front + 1) mod n을 활용

```python
def deQueue():
    global front
    if isEmpty():
        print("Queue is Empty")
    else:
        front = (front + 1) % len(Queue)
        return Queue[front]
```

## 문제 예시

주어진 N(2<= N <=100)개의 수를 순서대로 Queue에 넣은 후 하나씩 꺼내 화면에 출력하시오.
|Input|Output|
|---|---|
2 // 테스트 케이스 수
5 // 데이터 크기
1 2 3 4 5|1 2 3 4 5
5
5 4 2 3 1|5 4 2 3 1

## 구현

```python
queue = []


def queueInit():
    global queue
    queue.clear()


def queueIsEmpty():
    global queue
    return len(queue) == 0


def queueEnqueue(value):
    queue.append(value)


def queueDequeue():
    return queue.pop(0)


def main():
    T = int(input())

    for test_case in range(1, T + 1):
        N = int(input())

        queueInit()
        l = list(input().split())

        for i in l:
            queueEnqueue(i)

        print("#%d" % test_case, end=' ')

        while not queueIsEmpty():
            value = queueDequeue()
            if value != None:
                print(value, end=' ')

        print()
```

````java
import java.util.Scanner;

class Solution {

	static final int MAX_N = 100;

	static int front;
	static int rear;
	static int queue[] = new int[MAX_N];

	static void queueInit()
	{
		front = 0;
		rear = 0;
	}

	static boolean queueIsEmpty()
	{
		return (front == rear);
	}

	static boolean queueIsFull()
	{
		if ((front + 1) % MAX_N == rear)
		{
			return true;
		}
		else
		{
			return false;
		}
	}

	static boolean queueEnqueue(int value)
	{
		if (queueIsFull())
		{
			System.out.print("queue is full!");
			return false;
		}
		queue[front] = value;
		front++;
		if (front >= MAX_N)
		{
			front = MAX_N;
		}

		return true;
	}

	static Integer queueDequeue()
	{
		if (queueIsEmpty())
		{
			System.out.print("queue is empty!");
			return null;
		}

		Integer value = new Integer(queue[rear]);

		rear++;
		if (rear >= MAX_N)
		{
			rear = MAX_N;
		}
		return value;
	}

	public static void main(String arg[]) throws Exception {
		Scanner sc = new Scanner(System.in);

		int T = sc.nextInt();

		for (int test_case = 1; test_case <= T; test_case++)
		{
			int N = sc.nextInt();

			queueInit();
			for (int i = 0; i < N; i++)
			{
				int value = sc.nextInt();
				queueEnqueue(value);
			}

			System.out.print("#" + test_case + " ");

			while (!queueIsEmpty())
			{
				Integer value = queueDequeue();
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
