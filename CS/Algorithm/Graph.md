# Graph

## Minimum Spanning Tree(MST)
- 그래프에서 최소 비용 문제
  - 모든 정점을 연결하는 간선들의 가중치의 합이 최소가 되는 트리
  - 두 정점 사이의 최소 비용 경로 찾기
- 신장 트리
  - n개의 정점으로 이루어진 무방향 그래프에서 n개의 정점과 n-1개의 간선으로 이루어진 트리
  - 사이클이 발생하지 않음
- 최소 신장 트리
  - 무방향 가중치 그래프에서 신장 트리를 구성하는 간선들의 가중치의 합이 최소인 신장 트리

### Prim
하나의 정점에서 연결된 간선들 중에서 하나씩 선택하면서 MST를 만들어 가는 방식(최적 해를 보장할 수 없음)
1. 임의 정점을 하나 선택해서 시작
2. 선택한 정점과 인접하는 정점들 중의 최소 비용의 간선이 존재하는 정점을 선택
3. 모든 정점이 선택될 때 까지 반복

서로소인 2개의 집합 정보를 유지
- tree vertices: MST를 만들기 위해 선택된 정점들
- nontree vertices: 선택되지 않은 정점들

![prim gif](https://s3.stackabuse.com/media/articles/graphs-in-python-minimum-spanning-trees-prims-algorithm-8.gif)
```python
from heapq import heappush, heappop


def prim(start):
    heap = []
    visited = [False] * V
    sum_weight = 0
    heap.heapq(heap, (0, start))
    while heap:
        weight, start = heap.heappop()  # 방문 가능한 간선 중 가중치가 가장 낮은 간선 찾고
        if visited[start]:  # 이미 방문한 노드라면
            continue
        visited[start] = True  # 방문 기록
        sum_weight += weight  # 총 가중치 갱신
        for end in range(V):  # 모든 노드들 중
            if graph[start][end] == 0:  # 현재 노드와 연결되어 있지 않으면
                continue
            if visited[end]:  # 방문한 적이 있으면
                continue
            heappush(pq, (graph[start][end], end))

    print(f"최소 비용: {sum_weight}")

```

### KRUSKAL
간선을 하나씩 선택해서 MST를 찾는 알고리즘
1. 최초, 모든 간선을 가중치에 따라 오름차순으로 정렬
2. 가중치가 가장 낮은 간선부터 선택하면서 트리를 증가시킴
  - 사이클이 존재하면 다음으로 가중치가 낮은 간선 선택
3. n-1개의 간선이 선택될 때 까지 2번 반복

![kruskal gif](https://s3.stackabuse.com/media/articles/graphs-in-python-minimum-spanning-trees-kruskals-algorithm-6.gif)
```python
import sys


def find_parent(n):
    child = n
    while child != parents[child]:
        child = parents[child]
    return child


def union(x, y):
    x = find_parent(x)
    y = find_parent(y)
    if x == y:
        return
    parents[min(x, y)] = max(x, y)

def kruskal():
    count = 0
    total = 0
    for s, e, w in edges:
        if find_parent(s) != find_parent(e):
            count += 1
            union(s, e)
            total += w
            if count == V:
                break
    return total


sys.stdin = open('input.txt')
T = int(input())
for t in range(1, T+1):
    V, E = map(int, input().split())
    edges = []
    for _ in range(E):
        edges.append(tuple(map(int, input().split())))
    edges.sort(key=lambda x: x[2])
    parents = [i for i in range(V+1)]
    print(f"#{t} {kruskal()}")
```

#### Kruskal 알고리즘은 최적의 해를 도출하는가?
일반적인 Greedy algorithm의 정당성을 증명하는 방식으로 증명 가능  
최소 스패닝 트리를 T라고 하고, 크루스칼 알고리즘으로 구현했을 때 포함되는 간선이 최소 스패닝 트리 T에는 포함되어 있지 않다고 가정  
만약 크루스칼 알고리즘이 선택한 간선이 정점 u와 v를 연결한다고 하면, T는 해당 간선이 아닌 다른 어떠한 경로로 u와 v를 연결하고 있다.  
이 경로에 포함된 간선 중 하나는 분명히 크루스칼이 선택한 이 간선의 가중치 보다 크다.  
그렇지 않으면 해당 경로를 구성하는 간선들이 크루스칼 알고리즘에 의해 이전에 모두 선택되어 이미 u와 v를 연결하는 경로가 존재한다는 모순에 빠진다.  

만약 지금 선택된 간선보다 큰 가중치를 가지는 놈을 제거하고 크루스칼이 현재 선택한 간선으로 대체를 한다해도 T가 이미 최소 스패닝 트리이기 때문에 스패닝 트리임을 증명할 수 있다.  
출처: 구종만, 프로그래밍 대회에서 배우는 알고리즘 문제 해결 전략

## 최단 경로
간선의 가중치가 있는 그래프에서 두 정점 사이의 경로들 중에 간선의 가중치의 합이 최소인 경로
- 하나의 시작 정점에서 끝 정점까지의 최단경로
  - dijkstra 알고리즘
    - 음의 가중치를 허용하지 않음
  - bellman-ford 알고리즘
    - 음의 가중치 허용
- 모든 정점들에 대한 최단 경로
  - floyd-warshall 알고리즘

### Dijkstra
시작 정점에서 거리가 최소인 정점을 선택해 나가면서 최단 경로를 구하는 방식  
시작 정점에서 끝 정점까지의 최단 경로에 정점 x가 존재  
이때, 최단경로는 s에서 x까지의 최단 경로와 x에서 t까지의 최단 경로로 구성된다.  
![dijkstra](https://memgraph.com/images/blog/graph-search-algorithms-developers-guide/dijkstra.gif)

```python
from heapq import heappush, heappop


def dijkstra(start):
    heap = []
    heappush(heap, start)
    distance[start] = 0
    while heap:
        now = heappop(heap)
        if now not in graph:
            continue
        for next_node in graph[now]:
            new_dist = distance[now] + graph[now][next_node]
            if distance[next_node] <= new_dist:
                continue
            distance[next_node] = new_dist
            heappush(heap, next_node)

V, E = map(int, input().split())

graph = {}
distance = [float('inf')] * V

for _ in range(E):
    s, e, w = map(int, input().split())
    if s in graph:
        graph[s][e] = w
    else:
        graph[s] = {e: w}
```

### Bellman-Ford
특정 출발 노드에서 다른 모든 노드까지의 최단 경로 탐색  
음의 가중치 간선을 허용  
전체 그래프에서 음수 사이클의 존재 여부 판단 가능  
시간 복잡도: O(VE) (V: node 수, E: edge 수)
1. 출발 노드를 설정
2. 최단 거리 테이블 초기화
3. 다음와 과정을 노드의 개수 -1번 반복
  1. 전체 간선을 하나씩 확인
  2. 각 간선을 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블 갱신
    - 출발 노드가 방문한 적 없는 노드일 때 값을 갱신하지 않음
    - 출발 노드의 누적 거리값 + edge 가중치 < 종료 노드의 누적 거리값이면 종료 노드의 누적 거리값 갱신
4. 만약 음수 사이클의 존재 여부를 확인하고 싶다면 3번의 과정을 1번 더 수행
- 만약 최단 거리 테이블이 갱신된다면 음수 사이클이 존재하는 것  

*기본적인 개념은 dijkstra와 비슷하나 다른 모든 노드까지의 경로를 탐색해야 하기 때문에 연산 과정이 더 길어진 듯*
![bellman_ford](https://memgraph.com/images/blog/graph-search-algorithms-developers-guide/bellman-ford.gif)
```python
V, E = map(int, input().split())
edges = []
distance = [float('inf')] * (V + 1)

for _ in range(E):
    s, e, w = map(int, input().split())
    edges.append((s, e, w))

def bellman_ford(start):
    distance[start] = 0
    for i in range(V):
        # 매 반복마다 모든 간선 확인
        for j in range(E):
            cur_node = edges[j][0]
            next_node = edges[j][1]
            weight = edges[j][2]
            # 현재 간선을 거쳐서 다른 노드로 이동하는 거리가 더 짧은 경우 갱신
            if distance[cur_node] != float('inf') and distance[cur_node] + weight < distance[next_node]:
                distance[next_node] = distance[cur_node] + weight
                # n번째 라운드에서도 값이 갱신된다면 음수 순환이 존재
                if i == V - 1:
                    return True
    return False
```

### Floyd-Warshall
모든 노드 간 최단 경로를 구하는 알고리즘  
음의 가중치 간선 허용  

1. 하나의 정점에서 다른 정점으로 바로 갈 수 있으면 최소 비용을, 갈 수 없다면 INF값을 저장
2. 3중 for문을 통해 거쳐가는 정점을 설정한 후 해당 정점을 거쳐서 비용이 줄어드는 경우 값을 갱신

```python
def floyd_warshall():
    for via in range(V):
        for start in range(V):
            for end in range(V):
                if dist[start][via] + dist[via][end] < dist[start][end]:
                    dist[start][end] = dist[start][via] + dist[via][end]


V, E = map(int, input().split())
dist = [[float('inf') for _ in range(V)] for _ in range(V)]
for _ in range(E):
    s, e, w = map(int, input().split())
    dist[s][e] = w
```

## 출처
[memgraph](https://memgraph.com/blog/graph-search-algorithms-developers-guide)  
[mjieun.log](https://velog.io/@mjieun/Algorithm-%EB%B2%A8%EB%A7%8C-%ED%8F%AC%EB%93%9C-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98Bellman-Ford-algorithm-Python)