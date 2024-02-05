# Search

## Search
저장되어 있는 자료 중 원하는 자료를 찾는 것
- 탐색 키: 자료를 구별하여 인식할 수 있는 키

### 완전 탐색
- Brute-Force, 혹은 generate-and-test라고도 부름
- 모든 경우의 수를 테스트한 후, 최종 해답을 도출
- 경우의 수가 작을 때 유용
- 모든 경우의 수를 테스트하기 때문에 수행 속도는 느리지만, 해답을 찾아낼 확률이 높음
- 문제의 마땅한 풀이 방법이 떠오르지 않으면 우선 완전 탐색으로 접근하고 추후 다른 알고리즘을 활용하여 성능을 개선하는 것이 바람직함

[예시: 숫자 야구](https://www.acmicpc.net/problem/2503)

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


### KMP 알고리즘
- 불일치가 발생한 스트링의 앞 부분에 어떤 문자가 있는지 미리 알고 있으므로, 불일치가 발생한 앞 부분에 대해 다시 비교하지 않고 매칭을 진행
<!-- gif 찾아서 넣어놓기 -->
#### 시간 복잡도
O(M+N)
### 보이어 무어 알고리즘(Boyer-Moore algorithm)
<!-- gif 찾아서 넣어놓기 -->