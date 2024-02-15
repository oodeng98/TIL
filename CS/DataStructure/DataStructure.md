# Data Structure

### Stack
물건을 쌓아 올리듯 자료를 쌓아 올린 형태의 자료구조
- 스택에 저장된 자료는 선형 구조를 가짐
- 마지막에 삽입한 자료를 가장 먼저 꺼냄(LIFO: Last In First Out)

#### 구현
자료를 선형으로 저장할 저장소 필요(배열 사용 가능)  
top: 스택에 마지막으로 삽입된 원소의 위치

#### 연산
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


### Queue(Linear)
스택과 마찬가지로 삽입과 삭제의 위치가 제한적인 자료구조
- 큐의 뒤에서는 삽입만 하고, 큐의 앞에서는 삭제만 이루어지는 구조
- 먼저 삽입된 원소는 먼저 삭제됨(FIFO: First In First Out)

#### 구현
자료를 선형으로 저장할 저장소 필요
Front: 저장된 원소 중 첫번째 원소(혹은 삭제된 위치를 의미)
Rear: 저장된 원소 중 마지막 원소

#### 연산
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


### Circular Queue
선형 큐 이용시 원소의 삽입과 삭제를 계속할 경우, 배열의 앞부분에 활용할 수 있는 공간이 있음에도 불구하고 rear == n-1인 포화상태로 인식하여 더 이상의 삽입이 불가능해짐  
간단한 해결방법으로 매 연산이 이루어질 때마다 저장된 원소들을 배열의 앞부분으로 모두 이동시키는 방법이 있지만, 원소 이동에 많은 시간이 소요되어 효율성이 급격히 떨어진다.
- 그래서 1차원 배열을 사용하되, 논리적으로 배열의 처음과 끝이 연결된 원형 형태의 큐를 이룬다고 가정하고 사용

#### index의 순환
front와 rear의 위치가 배열의 마지막 인덱스인 n-1을 가리킨 후, 배열의 처음 인덱스인 0으로 이동해야 함  
이를 위해 나머지 연산자 mod 활용

#### 연산
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