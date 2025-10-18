# Bellman-Ford Algorithm

- 단일 시작 정점으로부터 모든 정점까지의 최단 경로를 찾는 알고리즘
- **음수 가중치 간선이 포함된 그래프**에서도 동작 가능
- **음수 사이클의 존재 여부** 판별 가능



## 전제 조건
- 그래프는 **방향 그래프**
- 간선 가중치는 음수일 수 있으나, **음수 사이클이 없어야** 최단 경로를 구할 수 있다.


## 절차
1. 거리 배열 초기화
    - 시작 정점의 거리는 0, 나머지 정점의 거리를 무한대로 설정
2. 모든 간선을 반복하여 완화
    - 모든 간선을 하나씩 확인하면서, 각 간선을 통해 더 짧은 경로가 발견되는 경우 거리를 갱신
    - 이 작업을 **(총 간선 - 1)번 반복**함
    - (총 정점 - 1)번인 이유는 시작 정점에서 모든 정점을 탐색하는데에 최대 (n - 1)회의 이동이 필요하기 때문
3. 음수 사이클 탐지
    - 모든 간선에 대해 1회 검사
    - 이 단계에서도 `dist[s] + d < dist[v]`가 참인 간선이 존재한다면, 이는 음수 사이클의 존재를 의미함
4. 최종 결과
    - 음수 사이클이 없다면, `dist[]` 배열에는 시작점에서 각 정점까지의 최단 거리가 저장됨

## 예시 코드
```python
def Bellman_Ford(n, edges, start):
    dist = [float('inf')] * (n+1)
    dist[start] = 0

    # 모든 간선을 (n-1)회 확인하며 간선 완화
    for i in range(n-1):
        for s, e, d in edges:
            if dist[e] != float('inf') and dist[s] + d < dist[e]:
                dist[e] = dist[s] + d
    
    # 음수 사이클 체크
    for s, e, d in edges:
        if dist[e] != float('inf') and dist[s] + d < dist[e]:
            return "Negative Cycle Detected"
    
    return dist[1:]
```