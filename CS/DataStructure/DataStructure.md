# Data Structure

[queue](#queuelinear)
[tree](#tree)

## Linked Queue

Linked List를 활용한 Queue

- Queue의 원소: 단수 연결 리스트의 노드
- Queue의 원소 순서: 노드의 연결 순서, 링크로 연결되어 있음

### 구현

초기 상태: front == rear == null
공백 상태: front == rear == null

<!-- 추후 코드도 찾아서 추가할 것 -->

## Priority Queue

우선순위를 가진 항목들을 저장하는 Queue  
FIFO 순서가 아니라 우선순위가 높은 순서대로 나가게 된다.

### 적용 분야

- 시뮬레이션 시스템
- 네트워크 트래픽 제어
- 운영체제의 테스크 스케쥴링

### 구현

<!-- 추후 추가할 것 -->

### Queue의 활용: Buffer(버퍼)

#### 버퍼란?

데이터를 한 곳에서 다른 한 곳으로 전송하는 동안 일시적으로 데이터를 보관하는 메모리의 영역

- 버퍼링: 버퍼를 활용하는 방식 또는 버퍼를 채우는 동작을 의미

버퍼는 일반적으로 입출력 및 네트워크와 관련된 기능에서 이용  
순서대로 입력/출력/전달되어야 하므로 FIFO방식의 자료구조인 Queue를 활용

## Tree

비선형 구조  
원소들 간에 1:n 관계를 가지는 자료구조  
원소들 간에 계층 관계를 가지는 계층형 자료구조

### 용어 정리

Root node: 노드 중 최상위 노드, 트리의 시작 노드  
node: 트리의 원소  
edge(간선): 노드를 연결하는 선  
sibling node: 같은 부모 노드의 자식 노드들  
parent nodes: 간선을 따라 루트 노드까지 이르는 경로에 있는 모든 노드들  
subtree: 부모 노드와 연결된 간선을 끊었을 때 생성되는 트리  
자손 노드: 서브 트리에 있는 하위 레벨의 노드들  
degree(차수): 노드에 연결된 자식 노드의 수  
트리의 차수: 트리에 있는 노드의 차수 중에서 가장 큰 값  
(Leaf node)단말 노드: 차수가 0인 노드, 자식 노드가 없는 노드  
높이: 루트에서 노드에 이르는 간선의 수, 레벨이라고도 함  
트리의 높이: 트리에 있는 노드의 높이 중 가장 큰 값, 최대 레벨이라고도 함

## Binary Tree(이진 트리)

모든 노드들이 2개의 서브트리를 갖는 형태의 트리  
각 노드가 자식 노드를 최대 2개까지만 가질 수 있음

### 구현1

이진 트리의 각 노드 번호를 아래와 같이 부여
![binary tree numbering](./img/binary_tree_numbering.png)

```python
[-, A, B, C, D, E, F, G, H, I, J, -, -, -, -, -] # 이 배열이 트리를 나타낸다.
```

노드 번호가 i인 노드 기준  
부모 노드의 번호: i // 2  
왼쪽 자식 노드의 번호: i _ 2  
오른쪽 자식 노드의 번호: i _ 2 + 1  
레벨 n의 노드 시작 번호: 2 \*\* n  
-> 하나의 배열로 트리를 구현할 수 있음  
-> 조상 찾기 또한 자식 노드의 번호 // 2 == 부모 노드의 번호임을 활용하여 찾을 수 있음

### 구현2

- 2개의 자식 번호를 저장하는 배열에 부모 번호의 인덱스로 자식 번호를 저장
- 자식 번호를 인덱스로 부모 번호를 저장

단점

- 사용하지 않는 배열 원소에 대한 메모리 공간 낭비
- 트리의 중간에 새로운 노드를 삽입하거나 기존의 노드를 삭제할 경우 배열의 크기 변경이 어려움  
  -> 연결 리스트를 활용한 이진 트리 구현

### Full Binary Tree(포화 이진 트리)

모든 레벨에 노드가 포화상태로 차 있는 이진 트리  
높이가 h일 때, 최대의 노드 개수인 2^(h+1) - 1의 노드를 가진 이진 트리

### Complete Binary Tree(완전 이진 트리)

높이가 h이고 노드 수가 n개일 때 포화 이진 트리의 노드 번호 1번부터 n번까지 빈 자리가 없는 이진 트리  
![complete binary tree](./img/complete_binary_tree.png)

#### heap(힙)

파이썬에서 자주 활용하는 heap이 완전 이진 트리의 대표적인 예시

```python
def enq(n):
    global last
    last += 1  # 마지막 노드 추가(완전이진트리 유지)
    h[last] = n  # 마지막 노드에 데이터 삽입
    child = last
    parent = child // 2
    while parent and h[parent] < h[child]:
        h[parent], h[child] = h[child], h[parent]
        child = parent
        parent = child // 2


def deq():
    global last
    temp = h[1]
    h[1] = h[last]
    last -= 1
    parent = 1
    child = 2 * parent
    while child <= last:
        if child + 1 <= last and h[child] < h[child+1]:
            child += 1
        if h[parent] < h[child]:
            h[parent], h[child] = h[child], h[parent]
            parent = child
            child = parent * 2

N = 10  # 필요한 노드 수
h = [0 for _ in range(N+1)]  # 최대 힙
last = 0  # 힙의 마지막 노드 번호

# 나중에 heapq 코드 뜯어서 확인해볼 것
```

### Skewed Binary Tree(편향 이진 트리)

높이 h에 대한 최소 개수의 노드를 가지면서 한쪽 방향의 자식 노드만들 가진 이진 트리  
![skewed binary tree](./img/skewed_binary_tree.png)

### Traversal(순회)

Traversal(순회): 트리의 노드들을 체계적으로 방문하는 것  
전위, 중위, 후위는 부모 노드의 방문 순서와 관련있음  
_코드에서 visit의 위치를 보면 도움이 될 지도_

- preorder(전위 순회): 부모 노드 방문 후, 자식 노드를 좌우 순서로 방문

```python
def preorder(node):
    if node:
        visit(t)  # 방문 체크
        preorder(node.left)
        preorder(node.right)
```

- inorder(중위 순회): 좌측 자식노드, 부모 노드, 우측 자식 노드 순서로 방문

```python
def inorder(node):
    if node:
        inorder(node.left)
        visit(t)  # 방문 체크
        inorder(node.right)
```

- postorder(후위 순회): 자식 노드를 좌우 순서로 방문 후, 부모 노드를 방문

```python
def postorder(node):
    if node:
        postorder(node.left)
        postorder(node.right)
        visit(t)  # 방문 체크
```
