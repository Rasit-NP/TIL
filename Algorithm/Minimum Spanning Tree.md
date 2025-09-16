# Minimum Spanning Tree

## 개요
- 가중 무향 그래프에서 모든 정점을 연결하는 부분 그래프 중 가중치의 합이 최소인 트리
- 값이 최소인 연결 구조를 찾는 문제로, 네트워크 설계, 클러스터링, 근사 알고리즘의 하위 루틴 등에서 핵심적으로 사용됨

## 기본 아이디어
1. 정의
    - 그래프 G에서 MST는 V의 모든 정점을 포함하고 사이클이 없는 연결 부분 집합이며, 가중치의 합이 최소임
2. 성질
    - 어떤 MST와 그래프의 임의의 cut을 고려하면, 그 컷을 가르는 가장 가벼운 간선은 어떤 MST에도 포함될 수 있음
    - 이 cut property와 cycle property가 Kruskal/Prim의 정당성을 보장
3. 대표 알고리즘
    - Kruskal
        - 모든 간선을 가중치 오름차순으로 정렬하고, 사이클을 만들지 않는 간선을 순서대로 선택
        - 사이클 판별은 Union-Find(Disjoint Set)로 처리
        - 주로 간선 수 E가 작은 그래프에서 효율적
    - Prim
        - 임의의 시작 정점에서 출발해 현재 선택된 정점 집합에서 경계를 이루는 가장 작은 가중치 간선을 계속 추가
        - 우선순위 큐로 구현하면 효율적
        - 인접 리스트 기반으로 구현 시 밀집/희소 그래프 모두에 유리하게 사용 가능

## 예시 코드
1. Kruskal
    ```python
    # 입력: n(정점 수, 정점은 0, .., n-1), edges = [(w, u, v), ...]
    def kruskal(n, edges):
            edges = sorted(edges)
            parent = list(range(n))
            rank = [0]*n
            
            def find(x):
                    while parent[x] != x:
                            parent[x] = parent[parent[x]]
                            x = parent[x]
                    return x
            
            def union(a, b):
                    ra, rb = find(a), find(b)
                    if ra == rb:
                            return False
                    if rank[ra] < rank[rb]:
                            parent[ra] = rb
                    elif rank[ra] > rank[rb]:
                            parent[rb] = ra
                    else:
                            parent[rb] = ra
                            rank[ra] += 1
                    return True
            
            mst_weight = 0
            mst_edges = []
            for w, u, v in edges:
                    if union(u, v):
                            mst_weight += w
                            mst_edges.append((u, v, w))
                            if len(mst_edges) == n-1:
                                    break
            
            # 만약 연결이 안 된 그래프라면 mst_edges의 길이는 n-1보다 작음
            return mst_weight, mst_edges
    ```

2. Prim
    ```python
    import heapq

    # 입력: n, adj(인접 리스트: adj[u] = [(v, w), ...])
    def prim(n, adj. start = 0):
            visited = False*n
            heap = [(0, start, -1)]    # (weight, to, from)
            mst_weight = 0
            mst_edges = []
            
            while heap and len(mst_edges) < n-1:
                    w, v, frm = heapq.heappop(heap)
                    if visited[v]:
                            continue
                    visited[v] = True
                    if frm != -1:
                            mst_edges.append((frm, v, w))
                            mst_weight += w
                            
                    for to, wt in adj[v]:
                            if not visited[to]:
                                    heapq.heappush(heap, (wt, to, v))
            
            # 연결되지 않은 경우 visited에 False가 남아있음
            if not all(visited):
                    return None, []
            return mst_weight, mst_edges
    ```

## 활용 팁 및 주의 사항
- 알고리즘 선택
    - 간선이 매우 많고 인접 행렬/밀집 표현을 쓰는 경우
        - Prim의 $O(V^2)$ 구현이 단순하고 빠름
    - 희소 그래프($E << V^2$)의 경우
        - Kruskal의 $O(E logE)$ 또는 Prim(힙)의 $O(E logV)$가 적합
    - 실무에서는 Prim(힙)과 Kruskal의 성능 차이는 구현 상 상수 및 자료구조에 따라 달라짐
- 연결성 검사
    - MST는 연결 그래프에서만 존재
    - 비연결 그래프는 최소 스패닝 포레스트(Minimum Spanning Forest)를 반환할 수 있음
- 동일한 가중치
    - 동일 가중치가 있을 때 MST는 여러 개가 존재할 수 있음
    - 문제가 특정 MST 구조를 요구하면 알고리즘 선택/간선 정렬 기준을 신경 써야 함
- 정확성 검증
    1. 선택한 간선의 개수가 n-1인지
    2. 사이클이 없는지
    3. 모든 정점이 연결되는지
    4. 총 가중치가 최솟값인지 확인

## 추가 정보
- 응용
    - 네트워크, 이미지 세그멘테이션, 근사 알고리즘 등
- 변형
    - 최대 신장 트리(Maximum Spanning Tree)
        - 간선 가중치를 부호 반전하거나, 정렬을 내림차순으로 하면 얻을 수 있음