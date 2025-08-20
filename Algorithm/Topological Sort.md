# Topological Sort

## 개요
- 방향 비순환 그래프(DAG, Directed Acyclic Graph)의 정점들을 모든 간선의 방향을 유지하면서 선형 순서로 나열하는 기법
- “작업 ‘A’가 ‘B’보다 먼저 수행되어야 한다”와 같은 **선후관계 제약**이 주어졌을 때, 이를 모두 만족하는 작업 순서를 만드는 알고리즘

## 기본 알고리즘
1. 정의 조건
    - 그래프에 사이클이 포함되면 위상 정렬이 불가능함
2. 주된 방법
    - Kahn’s Algorithm (BFS, indegree 기반)
        - 각 정점의 진입차수(indegree)를 계산
        - indegree가 0인 정점들을 큐에 넣고 하나씩 꺼내 결과에 추가
        이 정점에서 나가는 간선들을 하나씩 제거, 또는 대상 정점의 indegree--
        그 결과 indegree가 0이 된 정점들을 큐에 추가
        - 위 과정을 반복
        - 처리한 정점 수가 전체 정점 수와 같지 않다면 사이클 존재
        - 특징
            - 사이클 감지와 동시에 순서 생성
            - 우선순위를 반영하려면 큐 대신 우선순위 큐를 사용
    - DFS 기반
        - 각 정점에 대해 방문하면서 모든 자식을 먼저 방문한 뒤, 정점 방문이 끝나면 스택에 push
        - 모든 정점 처리 후 스택을 뒤집으면 위상 정렬 결과가 됨
        - 특징
            - 재귀 깊이에 의존
            - 사이클이 있으면 재귀 스택에서 발견 가능(방문 상태로 표시)
3. 복수의 정답 가능성
    - DAG에 대해 위상 정렬 결과는 유일하지 않을 수 있음
    - Kahn 알고리즘을 실행할 때, 큐에 동시에 여러 정점이 존재하면 가능한 순서가 여러 개 있다는 뜻
    - 위상 정렬이 유일하려면 Kahn 실행 중 항상 큐의 정점이 **정확히 하나**여야 함
4. 시간/공간 복잡도
    - 시간: O(V+E) → 모든 정점과 간선을 한 번씩 처리
    - 공간: O(V+E) → 인접 리스트, indegree 배열 등

## 예시 코드
1. Kan's Algorithm
    ```python
    from collections import deque

    def topo_kahn(n, edges):
            
            g = [[] for _ in range(n)]
            indeg = [0]*n
            for u, v in edges:
                    g[u].append(v)
                    indeg[v] += 1
            
            q = deque([i for i in range(n) if indeg[i] == 0])
            order = []
            while q:
                    u = q.popleft()
                    order.append(u)
                    for v in g[u]:
                            indeg[v] -= 1
                            if indeg[v] == 0:
                                    q.append(v)
            
            if len(order) != n:
                    return None
            return order
    ```

2. Priority Queue
    ```python
    import heapq

    def topo_lexicographic(n, edges):
            g = [[] for _ in range(n)]
            indeg = [0]*n
            for u, v in edges:
                    g[u].append(v)
                    indeg[v] += 1
            
            heap = [i for i in range(n) if indeg[i] == 0]
            heapq.heapify(heap)
            order = []
            while heap:
                    u = heapq.heappop(heap)
                    order.append(u)
                    for v in g[u]:
                            indeg[v] -= 1
                            if indeg[v] == 0:
                                    heapq.heappush(heap, v)
            
            if len(order) != n:
                    return None
            return order
    ```

3. DFS
    ```python
    import sys
    sys.setrecursionlimit(10**6)

    def topo_dfs(n, edges):
            g = [[] for _ in range(n)]
            for u, v in edges:
                    g[u].append(v)
            
            visited = [0]*n
            order = []
            cycle = False
            
            def dfs(u):
                    nonlocal cycle
                    if cycle:
                            return
                    visited[u] = 1
                    for v in g[u]:
                            if visited[v] == 0:
                                    dfs(v)
                            elif visited[v] == 1:
                                    cycle = True
                                    return
                    visited[u] = 2
                    order.append(u)
            
            for i in range(n):
                    if visited[i] == 0:
                            dfs(i)
            
            if cycle:
                    return None
            order.reverse()
            return order
    ```

## 활용 팁 및 주의사항
- 사이클 검사
    - Kahn 알고리즘으로 생성된 순서의 길이가 n보다 작으면 사이클이 존재하는 것
    - DFS에서는 방문 상태를 통해 백엣지로 사이클을 감지 가능
- 사전 순으로 가장 앞서는 위상 정렬을 원하면 우선순위 큐를 사용
- 메모리/성능
    - 인접 행렬을 사용하면 메모리/시간 낭비
    - 대부분의 경우에 인접 리스트를 사용
    - 큰 그래프에서는 generator나 메모리 효율적 자료구조를 고려
- 병렬/부분적 실행
    - 위상 정렬은 DAG의 partial order를 선형화하므로, 실제 작업 스케줄링에서 병렬 실행 가능한 작업 그룹을 찾아 병렬화하면 성능 개선 가능

## 추가 정보
- 응용 예시
    - 빌드 시스템(의존성 있는 소스파일 컴파일 분석)
    - 패키지 관리자(의존성 해결)
    - 컴파일러 최적화(의존성 기반 재배치)
- 대체/연관 개념
    - 위상 정렬은 partial order의 linear extension 문제와 동치