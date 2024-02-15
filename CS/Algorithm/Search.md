# Search

## Search
저장되어 있는 자료 중 원하는 자료를 찾는 것
- 탐색 키: 자료를 구별하여 인식할 수 있는 키

[Brute Force](#brute-force완전-탐색)  
[Sequential Search](#sequential-search순차-탐색)  
[Binary Search](#binary-search이진-탐색)  
[KMP](#kmpknuth-morris-pratt-알고리즘)  
[Boyer Moore](#boyer-moore보이어-무어-알고리즘)
[Depth First Search](#dfsdepth-first-search)
[Breadth First Search](#bfsbreadth-first-search)

### Brute Force(완전 탐색)
- generate-and-test라고도 부름
- 모든 경우의 수를 테스트한 후, 최종 해답을 도출
- 경우의 수가 작을 때 유용
- 모든 경우의 수를 테스트하기 때문에 수행 속도는 느리지만, 해답을 찾아낼 확률이 높음
- 문제의 마땅한 풀이 방법이 떠오르지 않으면 우선 완전 탐색으로 접근하고 추후 다른 알고리즘을 활용하여 성능을 개선하는 것이 바람직함

[예시: 숫자 야구](https://www.acmicpc.net/problem/2503)

### Sequential Search(순차 탐색)
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

### Binary Search(이진 탐색)
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


### KMP(Knuth Morris Pratt)  알고리즘
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

### Boyer Moore(보이어 무어) 알고리즘
패턴의 오른쪽 끝에 있는 문자가 불일치하고 이 문자가 패턴 내에 존재하지 않는 경우 패턴의 길이만큼 이동
- 오른쪽에서 왼쯕으로 비교
- 대부분의 상용 소프트웨어에서 채택하고 있는 알고리즘
#### 검색 과정
![Boyer-Moore gif](https://1104616303-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MTi055M1rv010z1nQle%2Fuploads%2Fgit-blob-3a3ddea1cb8187bd27cc541c64c2c3fc70485196%2FGIF%202021-02-19%20%EC%98%A4%ED%9B%84%2012-10-53.gif?alt=media)
```python
# 나중에 추가할 것
```
<!-- gif 찾아서 넣어놓기 -->

### DFS(Depth First Search)
#### 검색 과정
1. 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 탐색
2. 더 이상 갈 곳이 없으면 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 복귀
3. 다른 갈림길로 갈 수 있는 경로까지 탐색
4. 앞선 과정을 반복하여 모든 정점을 방문  

**가장 마지막에 만났던 갈림길의 정점으로 되돌아가야 하므로 후입선출(FILO) 구조의 스택 활용**

![DFS gif](https://velog.velcdn.com/images%2Fhiminhee%2Fpost%2F6a5f8969-1d9f-4df3-a5ba-33f8fa5e3ca4%2FDFS.gif)
```python
def dfs(graph, start_node):
    visit = {}
    '''
    보통 node의 이름은 순차적인 번호로 주어지므로, [0 for _ in range(n+1)]로 설정하기도 한다.
    또한 list가 아니라 dict로 설정하면 in 연산에서 시간을 많이 절약할 수 있다.
    '''
    stack = []
    stack.append(start_node)
    while stack:  # stack이 비면 더는 방문할 곳이 없으므로 break
        node = stack.pop()
        if node not in visit:  # 방문 체크를 여기서 할 수도 있고
            visit[node] = 0
            stack.extend(graph[node])  # 여기서 방문 체크를 해도 상관없다. 다만 extend 말고 append같은 함수를 써야겠지
```
### BFS(Breadth First Search)
#### 검색 과정
1. 탐색 시작점의 인접한 정점들을 모두 차례로 방문
2. 방문했던 정점으로 시작점으로 하여 다시 인접한 정점들을 차례로 방문

**인접한 정점들에 대해 탐색한 후 차례로 진행해야 하므로 선입선출(FIFO) 구조의 큐를 활용**

![BFS gif](https://velog.velcdn.com/images%2Fhiminhee%2Fpost%2F89922593-68b6-4805-a636-53bdfd312140%2FBFS.gif)
```python
def bfs(graph, start_node):
    visit = {}
    '''
    보통 node의 이름은 순차적인 번호로 주어지므로, [0 for _ in range(n+1)]로 설정하기도 한다.
    또한 list가 아니라 dict로 설정하면 in 연산에서 시간을 많이 절약할 수 있다.
    '''
    queue = []
    queue.append(start_node)
    while queue:  # queue가 비면 더는 방문할 곳이 없으므로 break
        node = queue.pop(0)  # 이처럼 pop(0)을 할거면 queue를 만들거나 deque를 활용하자
        if node not in visit:  # 방문 체크를 여기서 할 수도 있고
            visit[node] = 0
            queue.extend(graph[node])  # 여기서 방문 체크를 해도 상관없다. 다만 extend 말고 append같은 함수를 써야겠지
```

### 출처
[이진탐색 vs 순차탐색](https://www.mathwarehouse.com/)  
[KMP gif](https://velog.io/@junhok82/KMP)  
[보이어 무어 gif](https://til.hyunjin.space/algorithm/boyer-moore-horspool-algorithm)  
[DFS, BFS gif](https://velog.io/@himinhee/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-DFS%EA%B9%8A%EC%9D%B4-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89-BFS%EB%84%88%EB%B9%84-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89)