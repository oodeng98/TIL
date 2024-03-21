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
하나의 정점에서 연결된 간선들 중에서 하나씩 선택하면서 MST를 만들어 가는 방식
1. 임의 정점을 하나 선택해서 시작
2. 선택한 정점과 인접하는 정점들 중의 최소 비용의 간선이 존재하는 정점을 선택
3. 모든 정점이 선택될 때 까지 반복

서로소인 2개의 집합 정보를 유지
- tree vertices: MST를 만들기 위해 선택된 정점들
- nontree vertices: 선택되지 않은 정점들

```python
import heapq


def prim(start):
    heap = []
    visited = [False] * V
    sum_weight = 0
    heap.heapq(heap, (0, start))
    while heap:
        weight, frm = heap.heappop()
        sum_weight += weight
        visited[frm] = True
        for to in range(V):
            if graph[frm][to] != 0 and not MST[to]:
                heappush(pq, (graph[frm][to], to))

    print(f"최소 비용: {sum_weight}")
```

### KRUSKAL
간선을 하나씩 선택해서 MST를 찾는 알고리즘
1. 최초, 모든 간선을 가중치에 따라 오름차순으로 정렬
2. 가중치가 가장 낮은 간선부터 선택하면서 트리를 증가시킴
  - 사이클이 존재하면 다음으로 가중치가 낮은 간선 선택
3. n-1개의 간선이 선택될 때 까지 2번 반복


```python
def kruskal():
    pass


def find_set(n):
    if parents[n] != n:
        parents[n] = find_set(parents[n])
    return parents[x]


def union(x, y):
    x = find_set(x)
    y = find_set(y)
    if x == y:
        return
    parents[min(x, y)] = max(x, y)


V, E = map(int, input().split())
edges = []
for _ in range(E):
    s, e, w = map(int, input().split())
    edges.append((s, e, w))
edges.sort(key=lambda x: x[2])
parents = [i for i in range(V)]
sum_weight = 0


for s, e, w in edges:
    if find_set(s) == find_set(e):
        union(s, e)
        sum_weight += w
```

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


```python
from heapq import heappush, heappop


def dijkstra():
    heap = []
    heappush(heap, 0)
    distance[0] = 0
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
start = 0

graph = [[] for _ in range(V)]
distance = [float('inf')] * V

for _ in range(E):
    s, e, w = map(int, input().split())
    graph[s].append((w, e))
```

### Bellman-Ford

### Floyd-Warshall