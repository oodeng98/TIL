# Algorithm

### [Sorting](./Sorting.md)

### 완전 탐색
- Brute-Force, 혹은 generate-and-test라고도 부름
- 모든 경우의 수를 테스트한 후, 최종 해답을 도출
- 경우의 수가 작을 때 유용
- 모든 경우의 수를 테스트하기 때문에 수행 속도는 느리지만, 해답을 찾아낼 확률이 높음
- 문제의 마땅한 풀이 방법이 떠오르지 않으면 우선 완전 탐색으로 접근하고 추후 다른 알고리즘을 활용하여 성능을 개선하는 것이 바람직함

[예시: 숫자 야구](https://www.acmicpc.net/problem/2503)

### Greedy(탐욕) 알고리즘
- 여러 경우 중 최적이라고 생각되는 것을 선택하는 방식
- 각 시점에서 최적을 결정했다고 해서 최종 해답이 최적이라는 보장은 없음

#### 동작 과정
1. 해 선택: 현재 상태에서 최적 해를 구한 뒤, 이를 부분해 집합(Solution Set)에 추가
2. 실행 가능성 검사: 새로운 부분해 집합이 실행 가능한지 확인
3. 해 검사: 새로운 부분해 집합이 문제의 해가 되는지 확인  

[예시: 거스름돈 줄이기](https://www.acmicpc.net/problem/5585)


## Search
저장되어 있는 자료 중 원하는 자료를 찾는 것
- 탐색 키: 자료를 구별하여 인식할 수 있는 키

### Sequential Search
순차 탐색
- 가장 간단하고 직관적인 검색 방법
- 배열이나 연결 리스트 등 순차적으로 구현된 자료구조에서 유용
- 알고리즘이 단순하고 구현이 쉽지만, 검색 대상의 수가 많은 경우 비효율적

#### 정렬되어 있지 않은 경우
1. 첫번째 원소부터 검색 대상과 키 값이 같은 원소가 있는지 비교
2. 키 값이 같은 원소를 찾으면 해당 원소의 인덱스를 반환
- 자료의 마지막에 이를 때 까지 검색 대상을 찾지 못하면 검색 실패

#### 정렬되어 있는 경우
- 오름차순 정렬 가정
1. 자료를 순차적으로 검색하면서 키 값을 비교
2. 원소의 키 값이 검색 대상의 키 값보다 커지면 검색 종료

### Binary Search
이진 탐색
- 자료 가운데에 있는 원소의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 진행하는 방법
- **이진 탐색을 하기 위해선 자료가 정렬된 상태여야 한다**

#### 검색 과정
- 오름차순 정렬 가정
1. 자료의 중앙에 있는 원소 선택
2. 중앙 원소의 값과 목표 값의 키 값 비교
3. 목표 값의 키 값이 원소의 값보다 작으면 자료의 왼쪽에서 탐색 진행, 크면 자료의 오른쪽에서 탐색 진행
4. 앞선 과정 반복

```python
def binarySearch(data, low, high, key):
    if low > high: # 검색 실패
        return False
    middle = (low + high) // 2
    if key == data[middle]:
        return True
    elif key < data[middle]:
        return binarySearch(data, low, middle-1, key)
    elif data[middle] < key:
        return binarySearch(data, middle+1, high, key)

```
#### 이진탐색 vs 순차탐색
![시간 비교](https://www.mathwarehouse.com/programming/images/binary-vs-linear-search/binary-and-linear-search-animations.gif)
![Best Case](https://www.mathwarehouse.com/programming/images/binary-vs-linear-search/linear-vs-binary-search-best-case.gif)

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
