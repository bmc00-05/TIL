# 최소 비용

## MST

- 전체트리: 모든 정점을 연결 → 사이클 없는 여러 부분 그래프를 생성 가능 → 신장 트리
- 신장 트리에서 가중치가 사용
- 튜플 형태로 (도착점, 가중치) 형태로 저장
- 가중치를 저장하는 간선 리스트 형성
    
    → 최소 거리나 비용을 나타냄
    

## Prim

- BFS 처럼 정점을 기준으로 접근 가능 후보군 중 작은 값 탐색
- 큐에서 가중치가 적은 노드를 먼저 꺼낸다 → 우선순위 큐 →힙큐 O(logN)
- (가중치, 노드) 형태로 저장 → 우선순위 큐의 정렬 기준이 앞의 값을 우선 함
- 방문처리를 꺼냈을 때 실시
- 출발 정점을 바꾸어도 최소 비용은 동일하다 단 신장트리의 모양은 다를 수 있다.
- 시간 복잡도 ElogV

```python
# import heapq
from heapq import heappush, heappop

# 특정 정점 기준으로 시작
# - 갈 수 있는 노드들 중 가중치가 가장 작은 노드부터 간다
# --> 작은 노드를 먼저 꺼내기 위해 우선순위큐(heapq)를 활용한다
def prim(start_node):
    pq = [(0, start_node)]  # (가중치, 노드) 형태
    MST = [0] * V  # visited 와 동일하다
    min_weight = 0  # 최소 비용

    while pq:
        weight, node = heappop(pq)  # 가장 작은 가중치
        
        # 이미 방문한 노드라면 continue
        if MST[node]:
            continue

        MST[node] = 1           # node 로 가는 최소 비용이 선택되었다
        min_weight += weight    # 누적합 추가

        for next_node in range(V):
            # 갈 수 없으면 continue
            if graph[node][next_node] == 0:
                continue

            # 이미 방문했으면 continue
            if MST[next_node]:
                continue

            # 원래 BFS 에서는 여기서 방문 처리  ->  최소 비용 x
            heappush(pq, (graph[node][next_node], next_node))

    return min_weight

V, E = map(int, input().split())
graph = [[0] * V for _ in range(V)]  # 인접행렬
                                     # [선택과제] 인접 리스트로 구현

for _ in range(E):
    start, end, weight = map(int, input().split())
    graph[start][end] = weight
    graph[end][start] = weight  # 양방향

result = prim(4)  # 출발 정점과 함께 시작
                # 출발 정점을 바꾸어도, 최소비용은 똑같다
                # 단, 그래프가 다르게 나올수는 있다.
print(f'최소 비용 = {result}')

```

## Kruskal

- 가장 작은 가중치 부터 탐색 → 사이클 발생 시 제거
1. 가중치 기준으로 간선들 정렬 → 최소값 골라 꺼냄
2. 사이클 발생시 선택 x → union find 
- 가중치 기준 오름차순 정렬 edges,sort(key=lambda x: x[2])
- 사이클이 아니라면 연결 → 같은 집합으로  만든
- 시간 복잡도 ElogE

```python
def find_set(x):
    if x == parents[x]:
        return x

    # 기본 코드
    # return find_set(parents[x])

    # 경로 압축
    # find_set(parents[x])  # 대표자가 return
    parents[x] = find_set(parents[x])
    return parents[x]

def union(x, y):
    rx = find_set(x)
    ry = find_set(y)

    if rx == ry:  # 사이클 발생
        return

    # 일정한 규칙으로 병합 (더 작은 수로)
    if rx < ry:
        parents[ry] = rx
    else:
        parents[rx] = ry

V, E = map(int, input().split())

# 1. 간선들을 가중치 기준으로 정렬
edges = []
for _ in range(E):
    start, end, weight = map(int, input().split())
    edges.append((start, end, weight))  # 간선들의 정보를 저장

# 가중치 기준 오름차순 정렬
edges.sort(key=lambda x: x[2])

# 2. 가중치가 작은 간선부터 순서대로 선택하자
#  - 사이클이 발생하면 고르지 말자!
#  - 언제까지 ?
#   - MST 가 완성될 때까지
#   - == V-1 개를 선택할 때 까지
cnt = 0     # 현재까지 선택한 간선의 수
result = 0  # 가중치의 합

parents = [i for i in range(V)]  # make_set

for u, v, w in edges:
    # 사이클이 아니라면
    #  - 연결 (같은 집합으로 만든다)
    #  - cnt += 1
    #  - cnt 가 V - 1 이라면 종료
    if find_set(u) != find_set(v):  # u, v 사이클이 아니라면
        print(u, v, w)
        union(u, v)
        cnt += 1
        result += w

        if cnt == V - 1:
            break

print(f'최소 비용 = {result}')
```

### 무얼 써야하나?

- 간선이 많을 수록 → 완전 그래프 일수록
    
    → prim 유리 시간 복잡도 ElogV
    
- 보통의 적당한 그래프 일수록
    
    → kruskal 유리 시간 복잡도 ElogE
    

# 최단 경로

가중치가 있는 그래프에서 두 정점 사이의 경로 중 간선의 가중치 합이 최소

## Dijkstra

누적 비용이 작은 곳으로만 가자

- 음의 가중치를 허용하지 않음 → 음의 가중치 O 벨만-포드
- 다른 모든 연결된 정점들까지의 최단 경로
- 모든 정점들에 대한 최단 경로 → 플로이드-워샬

```python
from heapq import heappop, heappush

def dijkstra(start_node):
    pq = [(0, start_node)]  # (누적거`리, 노드번호)
    dists = [INF] * V       # 각 정점까지릐 최단거리를 저장할 리스트
    dists[start_node] = 0   # 시작노드 최단거리는 0

    while pq:
        dist, node = heappop(pq)

        # 이미 더 작은 값으로 온 적이 있으면 버린다
        #  - (3,c)  (4,c) 예시
        if dists[node] < dist:
            continue

        for next_dist, next_node in graph[node]:
            # 다음 노드로 가기 위한 누적 거리
            # 누적 거리 = 현재까지의 거리 + 다음 거리
            new_dist = dist + next_dist

            # 이미 작거나 같은 가중치로 온 적이 있다면 continue
            if dists[next_node] <= new_dist:
                continue

            # 누적거리, 새로운 노드를 pq에 저장 + dists 에 갱신
            dists[next_node] = new_dist
            heappush(pq, (new_dist, next_node))

    return dists

INF = int(21e8)  # 무한대를 가정 (문제의 최대)
                 # 손으로 다 더했을 때 최대값이 몇일까?

V, E = map(int, input().split())
start_node = 0  # 시작점
graph = [[] for _ in range(V)]  # 인접 리스트로 구현
                                # [선택과제] 인접 행렬로 구현

for _ in range(E):
    start, end, weight = map(int, input().split())
    graph[start].append((weight, end))  # [주의] 단방향

# 출발지로부터 모든 최단거리
result = dijkstra(0)
print(result)
```

1. 누적 거리 리스트
2. 누적거리가 짧은 경로를 먼저 꺼냄 → 우선순위 큐 → 힙큐
3. 큐에서 꺼내는 순간 최단거리임이 확정된다. → 최단거리가 최소일때만 꺼내어 저장