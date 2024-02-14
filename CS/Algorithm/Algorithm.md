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

### 조합
```python
# itertools.permutations를 가져옴
def permutations(iterable, r=None):
    # permutations('ABCD', 2) --> AB AC AD BA BC BD CA CB CD DA DB DC
    # permutations(range(3)) --> 012 021 102 120 201 210
    pool = tuple(iterable)
    n = len(pool)
    r = n if r is None else r
    if r > n:
        return
    indices = list(range(n))
    cycles = list(range(n, n-r, -1))
    yield tuple(pool[i] for i in indices[:r])
    while n:
        for i in reversed(range(r)):
            cycles[i] -= 1
            if cycles[i] == 0:
                indices[i:] = indices[i+1:] + indices[i:i+1]
                cycles[i] = n - i
            else:
                j = cycles[i]
                indices[i], indices[-j] = indices[-j], indices[i]
                yield tuple(pool[i] for i in indices[:r])
                break
        else:
            return
```
#### 필자의 이해 및 설명

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
평소 순열을 만드는 과정을 생각해보면 간단한데, [1, 2, 3, 4]으로 순열을 만들어보자.
1. 첫번째 자리에 1을 넣는다
1-1. 
```python
# itertools.combinations를 가져옴
def combinations(iterable, r):
    # combinations('ABCD', 2) --> AB AC AD BC BD CD
    # combinations(range(4), 3) --> 012 013 023 123
    pool = tuple(iterable)
    n = len(pool)
    if r > n:
        return
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