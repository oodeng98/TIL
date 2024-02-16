# Algorithm

### [Sorting](./Sorting.md)
### [Search](./Search.md)

### Greedy(탐욕) 알고리즘
여러 경우 중 최적이라고 생각되는 것을 선택하는 방식
- 각 시점에서 최적을 결정했다고 해서 최종 해답이 최적이라는 보장은 없음

#### 동작 과정
1. 해 선택: 현재 상태에서 최적 해를 구한 뒤, 이를 부분해 집합(Solution Set)에 추가
2. 실행 가능성 검사: 새로운 부분해 집합이 실행 가능한지 확인
3. 해 검사: 새로운 부분해 집합이 문제의 해가 되는지 확인  

[예시: 거스름돈 줄이기](https://www.acmicpc.net/problem/5585)


### DP(Dynamic Programming)
입력 크기가 작은 부분 문제들을 해결한 후 그 해들을 이용하여 보다 큰 크기의 부분 문제들을 해결하여 최종적으로 원래 주어진 입력의 문제를 해결하는 알고리즘
- Greedy 알고리즘과 같이 최적화 문제를 해결하는 알고리즘
#### DP 예시

```python
# DP를 적용한 피보나치 알고리즘
def fibo(n):
    f = [0] * (n + 1)
    f[0] = 0
    f[1] = 1
    for i in range(2, n+1):
        f[i] = f[i-1] + f[i-2]
    return f[n]
```


### Divide and Conquer(분할 정복)
문제를 작은 문제로 분리하고 각각을 해결한 다음, 결과를 모아서 원래의 문제를 해결하는 방법
#### Divided and Conquer의 대표적인 알고리즘
[Quick Sort](./Sorting.md#quick-sort)


### 비트 연산을 활용한 부분집합 만들기
```python
def comb(arr, n):
    result = []
    for i in range(1<<n): # 부분 집합의 개수, 1<<n은 2**(n)과 같음
        temp = []
        for j in range(n):
            if i & (1<<j):
                temp.append(arr[j])
        result.append(temp)
    return result
```
#### 필자의 이해 및 설명
부분 집합의 개수는 2 ** n으로, 0부터 2 ** n까지 수를 모두 이진법으로 변환해서 생각해보자.  
중요한 것은 0 ~ 2 ** n까지의 모든 숫자는 각각 다른 이진법 표기 방식을 가진다는 점이다.  
즉, 숫자의 각각의 이진법 표기와 부분집합을 하나씩 매칭할 수 있다는 뜻이 된다.  
그중 이진수로 표기했을 때 1로 표기된 부분 각각을 오른쪽부터 센 비트 순서를 indexes라고 표현하겠다.  
그 후 부분 집합을 구하고자 하는 집합(위의 코드에서 arr을 의미)에서 indexes에 해당하는 값들을 가져오면 하나의 부분 집합을 구할 수 있고, 이를 반복하면 된다.


### 순열
```python
# 모든 원소를 사용한 순열
def f(i, k):
    if i == k:  # N개의 원소가 모두 배정된 경우
        print(*P)
    else:
        for j in range(i, k):
            P[i], P[j] = P[j], P[i]  # P[i]에는 i 이후의 모든 원소가 한번씩 왔다 감
            f(i+1, k)  #  i 좌측은 확정한 상태로 우측을 돌림
            P[i], P[j] = P[j], P[i]  # 교환 전으로 복구
N = 3
P = [1, 2, 3]
f(0, N)
```
#### 필자의 이해 및 설명
기본적으로 하나씩 확정하면서 진행하는 방식이다.  
i 왼쪽의 값들은 이미 확정된 상태이고, 오른쪽 값들과 i번째 값을 바꿔가면서 순열을 완성해간다.  
[yield란?](/Python/Python.md#yield-키워드)
```python
# itertools.combinations를 가져옴
def permutations(iterable, r=None):
    # permutations('ABCD', 2) --> AB AC AD BA BC BD CA CB CD DA DB DC
    # permutations(range(3)) --> 012 021 102 120 201 210
    pool = tuple(iterable)
    n = len(pool)
    r = n if r is None else r
    # if r is None:
    #     r = n
    # else:
    #     r = r
    # 위와 같은 뜻
    if r > n:  # 순열의 길이가 iterable의 길이보다 더 긴 경우
        return  # 불가능
    indices = list(range(n))  # [0, 1, ..., n-1]
    cycles = list(range(n, n-r, -1))  # [n, n-1, ..., n-r+1]
    # 여기서 cycles의 값인 [n, n-1, n-r+1]을 모두 곱하면 순열의 총 개수와 같다는 사실을 인지하고 넘어가자
    yield tuple(pool[i] for i in indices[:r])  # 첫번째로 (0, 1, ..., r) 반환
    while n:
        for i in reversed(range(r)):  # (r-1, r-2, ..., 0)
            cycles[i] -= 1
            if cycles[i] == 0:  # 만약 cycles[i]가 0이 되었다면
                indices[i:] = indices[i+1:] + indices[i:i+1]  # indices 원상복귀
                # indices = list(range(n))를 사용해도 결과는 동일하나 시간이 더 들긴 할 듯
                '''
                cycles[i]가 0이 되었다면 가장 최근까지 cycles[i]의 값은 1을 가리키고
                그렇다면 j는 -1이었으므로 indices[i]가 가리키는 값은 indices에서 가장 오른쪽 값인 indices[-1]이다.
                그래서 indices[i+1:]의 값 오른쪽에 indices[i]의 값인 indices[i:i+1]를 넣어서 indices를 원상복구 시키는것
                '''
                cycles[i] = n - i  # cycles[i] 또한 원상복귀
                '''
                cycles = [n, n-1, n-2, ..., n-(r-1)]
                현재 i는 r-1에서 cycles에 존재하는 0의 개수를 뺀 값
                왜냐? i의 값이 감소하려면(반복문이 break 되지 않고 돌려면) 뒤에 있는 cycles[i]의 값이 전부 0이어야 하기 때문
                '''
            else:  # cycles[i] == 0이 될 때 까지 아래 코드 수행
                j = cycles[i]  # j의 값은 점차 작아짐
                indices[i], indices[-j] = indices[-j], indices[i]
                '''
                indices[-j]가 가리키는 값은 오른쪽으로 한칸씩 계속 이동
                why? j = cycles[i]이고 cycles[i]는 매 반복문마다 -=1되기 때문
                그렇다면 indices[i]가 가리키는 값이 오른쪽으로 한칸씩 이동한다
                => 1씩 값이 증가한다.
                '''
                yield tuple(pool[i] for i in indices[:r])  # 모든 yield의 형태는 이와 같고, 이는 indices에서 앞선 r개의 원소만 본다는 뜻
                break
        else:
            return
```

#### 필자의 이해 및 설명
세세한 부분들에 대한 설명은 넘어가고, 첫 yield가 나오는 부분부터 설명하겠다.  
yield 반환하는 값의 형태 모두 아래와 같다.  
```python
tuple(pool[i] for i in indices[:r])
```
이는 indices에서 앞선 r개의 원소만 볼 것이고, 해당 원소들이 각각 pool의 index를 의미한다는 뜻이다.  
아래로 내려가서 while n은 iterable의 길이가 0이 아니면 계속 실행하게 만들고,   
```python
for i in reversed(range(r))
```
로 인해 i는 (r-1, r-2, ..., 0)의 값을 순차적으로 가진다.  
그로 인해 i는 cycles의 뒷 값부터 건드리게 된다.  

반복문이 시작되면 일단 아래 코드가 우선적으로 실행된다.
```python
cycles[i] -= 1
```
그리고 if else 구문에서 cycles[i]의 값이 0일때와 0이 아닐 때가 구분되는데, 우선 else의 조건인 0이 아닐 때 부터 보겠다.  

보기 편하게 j에 cycles[i]의 값을 대입,
indices[i]와 indices[j]의 자리를 바꿔준 후 yield로 값을 반환한다.  
이때 중요한 것은 마지막에 존재하는 break이다.  
cycles[i]가 0이 아닌 이상 반복문은 깨지고 다시 시작되므로 i는 r-1 잠시 고정된다.  
그렇다면 cycles[i]라는 바뀌는 자리는 고정이고, j는 반복문이 깨졌다가 시작할 때 마다 1씩 감소하므로 cycles[-j]는 점차 오른쪽을 가리키게 된다.  

이 과정을 r에 2를 대입한 코드의 결과를 보면서 이해해보자.
|indices|cycles|j|description|
|---------------|------|-|------------------------------|
|[0, 1, 2, 3, 4]|[5, 4]|-||
|[0, 2, 1, 3, 4]|[5, 3]|3||
|[0, 3, 1, 2, 4]|[5, 2]|2|아래 부분까지 cycles의 마지막 값이 점차 감소하는 것을 확인할 수 있다.|
|[0, 4, 1, 2, 3]|[5, 1]|1|그로 인해 indices[i]값이 점차 증가하는 것을 확인할 수 있다.|

이 위치에서 cycles의 마지막 값이 0이 되고, 처음으로 반복문이 깨지지 않고 돌아가게 된다.  
이 과정에서 처음으로 if문의 코드가 수행되게 되고 아래처럼 초기 값으로 원상복귀한다.
|indices|cycles|
|---------------|------|
|[0, 1, 2, 3, 4]|[5, 4]|

그리고 반복문이 수행되어 i가 가리키는 값이 1 줄어들고, 이후 cycles[i]는 지금까지와 달리 cycles의 왼쪽 값을 건드리게 된다.  
이제 cycles가 [4, 4]가 되고, 이젠 0이 아니므로 else구문으로 들어간다.  
그 과정에서 indices에서 i가 가리키는 값인 좌측 값이 처음으로 변화하게 된다.  
그래서 indices의 첫번째 값이 1로 변하고, 반복문이 break된다.  
이제 많이 본 과정을 볼 수 있다. 아래 표를 확인해보자.

|indices|cycles|j|description|
|---------------|------|-|------------------------------|
|[1, 0, 2, 3, 4]|[4, 4]|4|숫자가 바뀌었지만 구조는 똑같다.|
|[1, 2, 0, 3, 4]|[4, 3]|3|indices[i]의 값에 오른쪽 4개의 값을 한번씩 번갈아가며 넣어주고|
|[1, 3, 0, 2, 4]|[4, 2]|2|...|
|[1, 4, 0, 2, 3]|[4, 1]|1||
|||||
|[2, 0, 1, 3, 4]|[3, 4]|3|다시 cycles가 0이 되었으므로 indices의 값이 변화하고|
|[2, 1, 0, 3, 4]|[3, 3]|3|indices[i]의 값에 오른쪽 4개의 값을 한번씩 번갈아가며 넣어주는 과정을 반복한다.|
|[2, 3, 0, 1, 4]|[3, 2]|2||
|[2, 4, 0, 1, 3]|[3, 1]|1||
|||||
|[3, 0, 1, 2, 4]|[2, 4]|2||
|[3, 1, 0, 2, 4]|[2, 3]|3||
|[3, 2, 0, 1, 4]|[2, 2]|2||
|[3, 4, 0, 1, 2]|[2, 1]|1||
|||||
|[4, 0, 1, 2, 3]|[1, 4]|1||
|[4, 1, 0, 2, 3]|[1, 3]|3||
|[4, 2, 0, 1, 3]|[1, 2]|2||
|[4, 3, 0, 1, 2]|[1, 1]|1|앞선 과정들을 계속 반복하고, r=2가 아니어도 반복 과정은 같다.|

이 과정이 끝나면 cycles의 모든 값은 0이 되고, 반복문이 break를 만나지 않고 끝나게 된다.  
드디어 for else구문에서 else 구문으로 넘어갈 수 있고, 함수가 종료된다.  
여기서 cycles의 값이 모두 0이 될 때 까지 실행했으니 모든 과정은 결국 cycles의 값을 모두 곱한 만큼 실행하게 되고, 이는 사실 순열의 총 개수를 구한 공식과 같음을 알 수 있다.

### 조합
```python
# itertools.combinations를 가져옴
def combinations(iterable, r):
    # combinations('ABCD', 2) --> AB AC AD BC BD CD
    # combinations(range(4), 3) --> 012 013 023 123
    pool = tuple(iterable)
    n = len(pool)
    if r > n:  # 순열의 길이가 iterable의 길이보다 더 긴 경우
        return  # 불가능
    indices = list(range(r))
    yield tuple(pool[i] for i in indices)
    while True:
        for i in reversed(range(r)):
            if indices[i] != i + n - r:
                break
        else:
            return
        indices[i] += 1
        for j in range(i+1, r):
            indices[j] = indices[j-1] + 1
        yield tuple(pool[i] for i in indices)
```
#### 필자의 이해 및 설명


### 출처

[Math Warehouse](https://www.mathwarehouse.com/programming/gifs/binary-vs-linear-search.php)
[itertools](https://docs.python.org/ko/3/library/itertools.html#module-itertools)