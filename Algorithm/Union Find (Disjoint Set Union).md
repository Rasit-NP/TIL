# Union Find (Disjoint Set Union)

## 개요
- 서로서 집합 자료구조
- 여러 원소를 몇 개의 집합으로 관리하며 연산을 빠르게 수행
    1. 어떤 원소가 속한 집합의 대표를 찾는 find
    2. 두 원소의 집합을 합치는 union
- 그래프의 연결성 검사, Kruskal 알고리즘, 동치류 관리 등에 사용됨

## 기본 아이디어
- 각 원소는 트리 구조로 표현되고, 트리의 루트가 집합의 대표
- `find(x)`는 x의 루트를 찾는 함수
- `union(a, b)`는 a의 루트와 b의 루트를 연결
- 성능 향상을 위해 두 기법을 결합
    - 경로 압축 (Path Compression)
        - `find`할 때 탐색 경로의 모든 노드를 직접 루트에 연결해 다음 `find`를 빠르게 함
    - 합치기 기준 (Union by rank/size)
        - 더 작은 트리를 더 큰 트리의 자식으로 붙여 트리 높이를 제한
    - 이 두 기법을 쓰면 수행시간은 **아커만 역함수**에 비례
        - 실무에서는 O(1)처럼 동작

## 예시 코드
```python
def find(parent, x):
    root = x
    while parent[root] != root:
        root = parent[root]
    # 경로 압축
    while parent[x] != root:
        nxt = parent[x]
        parent[x] = root
        x = nxt
    return root

def union(parent, size, a, b):
		ra = find(parent, a)
		rb = find(parent, b)
		if ra == rb:
				return False
		if size[ra] < size[rb]:
				ra, rb = rb, ra
		parent[rb] = ra
		size[ra] += size[rb]
		return True

def same(parent, a, b):
		return find(parent, a) == find(parent, b)

def component_size(parent, size, x):
		return size[find(parent,x)]

# 예시
n = 6
parent = list(range(n))
size = [1] * n

union(parent, size, 0, 1)
union(parent, size, 2, 3)
union(parent, size, 1, 2)

print(same(parent, 0, 3))
# True

print(component_size(parent, size, 0))
#4
```

## 활용 팁 및 주의 사항
- 문제 유형
    - 동적 연결, Kruskal MST, 사이클 검출, 동치류 병합 등
- 문자열/복합키
    - 요소가 문자열이면 `dict`(python)이나 `unordered_map`(C++)로 정수 인덱스에 매핑해 사용
- 삭제 불가
    - 기본 DSU는 합병만 지원하고 분할은 지원하지 않음
    - 동적 간선 삭제가 필요하면 다른 기법이 필요
        - Offline Dynamic Connectivity
        - 분할 정복 트리 + Rollback DSU

## 추가 정보
- Rollback DSU
    - Offline 쿼리나 분할 정복 트리에 결합해 간선 추가/삭제 문제 해결
- DSU with parity
    - 이분 그래프 판별이나 관계 제약 관리 가능