# Algorithm

### [Sorting](./Sorting.md)
### [Search](./Search.md)

### Greedy(탐욕) 알고리즘
- 여러 경우 중 최적이라고 생각되는 것을 선택하는 방식
- 각 시점에서 최적을 결정했다고 해서 최종 해답이 최적이라는 보장은 없음

#### 동작 과정
1. 해 선택: 현재 상태에서 최적 해를 구한 뒤, 이를 부분해 집합(Solution Set)에 추가
2. 실행 가능성 검사: 새로운 부분해 집합이 실행 가능한지 확인
3. 해 검사: 새로운 부분해 집합이 문제의 해가 되는지 확인  

[예시: 거스름돈 줄이기](https://www.acmicpc.net/problem/5585)

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

### 출처

[Math Warehouse](https://www.mathwarehouse.com/programming/gifs/binary-vs-linear-search.php)
