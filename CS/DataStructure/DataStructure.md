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
<!-- 8page부터 다시 정리 -->