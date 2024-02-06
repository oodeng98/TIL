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


### KMP(Knuth-Morris-Pratt)  알고리즘
- 불일치가 발생한 스트링의 앞 부분에 어떤 문자가 있는지 미리 알고 있으므로, 불일치가 발생한 앞 부분에 대해 다시 비교하지 않고 매칭을 진행
- 패턴을 전처리하여 잘못된 시작을 최소화
#### 검색 과정
![LPS](https://miro.medium.com/v2/resize:fit:720/format:webp/1*OIb4erqMedwaze8aTUi9gw.gif)

![KMP gif](https://velog.velcdn.com/images/junhok82/post/f3d31545-01f3-43e0-87ad-bd607ec589f2/kmp.gif)
<!-- gif 찾아서 넣어놓기 -->


```python
def kmp(t, p):
    N = len(t)
    M = len(p)
    lps = [0] * (M+1)
    # lps 찾기
    j = 0
    lps[0] = -1
    for i in range(1, M):
        lps[i] = j
        if p[i] == p[j]:
            j += 1
        else:
            j = 0
    lps[M] = j
    print(lps)
    # search
    i = 0 # 비교할 텍스트 위치
    j = 0 # 비교할 패턴 위치
    while i < N  and j <= M:
        if j == -1 or t[i] == p[j]: # 첫글자가 불일치했거나, 일치하면
            i += 1
            j += 1
        else:
            j = lps[j]
        if j == M: # j가 len(p)와 같은 경우 -> 패턴을 찾은 경우
            print(i-M, end=' ') # t에서 패턴이 시작되는 index 출력
            j = lps[j]
    print()
    return

t = 'zzzabcdabcdabcefabcd'
p = 'abcdabcef'
kmp(t, p)
# result
# [-1, 0, 0, 0, 0, 1, 2, 3, 0, 0]
# 7
```
<!-- https://velog.io/@junhok82/KMP
https://towardsdatascience.com/pattern-search-with-the-knuth-morris-pratt-kmp-algorithm-8562407dba5b 참고해서 다시 정리 -->
#### 시간 복잡도
O(M+N)

### 보이어 무어(Boyer-Moore algorithm) 알고리즘
패턴의 오른쪽 끝에 있는 문자가 불일치하고 이 문자가 패턴 내에 존재하지 않는 경우 패턴의 길이만큼 이동
- 오른쪽에서 왼쯕으로 비교
- 대부분의 상용 소프트웨어에서 채택하고 있는 알고리즘
#### 검색 과정
![Boyer-Moore gif](https://1104616303-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MTi055M1rv010z1nQle%2Fuploads%2Fgit-blob-3a3ddea1cb8187bd27cc541c64c2c3fc70485196%2FGIF%202021-02-19%20%EC%98%A4%ED%9B%84%2012-10-53.gif?alt=media)
```python
# 나중에 추가할 것
```
<!-- gif 찾아서 넣어놓기 -->

### 출처

[이진탐색 vs 순차탐색](https://www.mathwarehouse.com/)  
[KMP gif](https://velog.io/@junhok82/KMP)  
[보이어 무어 gif](https://til.hyunjin.space/algorithm/boyer-moore-horspool-algorithm)