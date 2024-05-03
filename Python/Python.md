# Python

### yield 키워드

yield 키워드는 generator를 만드는 키워드

```python
def return_abc():
  return list("ABC")
```

```python
>>> print(return_abc())
['A', 'B', 'C']
```

```python
def yield_abc():
  yield "A"
  yield "B"
  yield "C"
```

```python
>>> print(yield_abc())
<generator object yield_abc at 0x7f4ed03e6040>
```

#### yield from

```python
def yield_abc(lst):
  for ch in lst:
    yield ch
```

위처럼 리스트를 제너레이터로 변환할 수도 있지만

```python
def yield_abc(lst):
  yield from lst
```

이렇게 편리하게 변환할 수 있다.

### generator란?

쉽게 말해 여러 개의 데이터를 미리 만들어 놓지 않고 필요할 때 마다 하나씩 만들어내는 객체

#### 왜 이렇게 사용해야 하는가?

generator를 활용하지 않으면 10000개의 데이터를 미리 만들고 코드가 수행되지만, generator의 경우 함수의 메모리 주소만 기억하고, 그때그때 생성해서 반환하기 때문에 메모리 공간에서 굉장한 이점을 얻을 수 있다.  
또한, 기존에는 첫번째 결과값을 10000번의 연산 이후에나 얻을 수 있었지만, generator를 활용할 경우 1번의 연산 후 첫번째 결과값을 얻을 수 있다.

#### generator comprehension

```python
abc = (ch for ch in "ABC")

print(abc)
for ch in abc:
  print(ch)
```

```python
<generator object <genexpr> at 0x7f2dab21ff90>
A
B
C
```

list comprehension과 굉장히 유사한데, 차이점은 list는 대괄호를 사용하고 제너레이터는 소괄호를 사용한다는 점이다.

#### generator 사용 예시

```python
def delta(x, y):
    for a, b in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
        if 0 <= x + a < N and 0 <= y + b < N:
            yield x + a, y + b
    return  # 없어도 상관없음

def delta(x, y):
    result = []
    for a, b in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
        if 0 <= x + a < N and 0 <= y + b < N:
            result.append(tuple(x + a, y + b))
    return  result

for i in delta(0, 0):
  print(i)
```

위 함수는 x, y좌표가 들어오면 해당 좌표의 상하좌우를 반환하는 코드이다.  
위처럼 함수를 작성하면 아래 for문에서 매번 하나의 값을 생성하여 출력하지만  
아래처럼 함수를 작성하면 result에 4개의 값이 저장된 상태로 실행되므로 메모리를 더 많이 차지함을 알 수 있다.
