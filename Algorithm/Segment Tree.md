# Segment Tree

## 개요
- 고정된 배열에서 구간에 대한 집계(합, 최솟값, 최댓값 등)을 로그 시간으로 질의하고, 점 또는 구간을 갱신할 수 있게 해 주는 자료구조
- 배열을 이진 트리 형태로 분할하여 각 노드에 해당 구간의 집계값을 저장
- 주로 빈번한 구간 질의와 갱신(online query/update)이 섞여 있는 문제에 사용

## 기본 아이디어
1. 분할 정복 구조
    - 전체 배열을 반 씩 나누어 트리 노드로 표현
    - 루트는 전체 구간을 담당하고, 잎 노드는 단일 원소를 담당
2. 노드 값
    - 각 노드는 자신이 담당하는 구간의 집계값(합, 최소 ,최대)을 저장
3. 질의(query)
    - 질의 구간이 현재 노드의 구간과 완전히 일치하면 그 값을 사용
    - 일부만 겹치면 자식 노드에서 필요한 부분을 재귀적으로 가져와 병합
4. 갱신(update)
    - 점 갱신
        - 한 원소의 값을 바꾸면 그 원소에 해당하는 잎부터 루트까지 올라가며 노드들을 갱신
    - 구간 갱신
        - 범위를 한꺼번에 갱신해야 할 때는 lazy propagation(지연 전파) 기법을 사용하여 전체 구간을 즉시 내려쓰지 않고 필요할 때만 적용
5. 시간 복잡도
    - 빌드 $O(n)$, 점 갱신 $O(log n)$, 구간 질의 $O(log n)$, 구간 갱신(지연 전파 사용) $O(log n)$

## 예시 코드
1. bottom-up segment tree - 점 갱신, 구간 합 질의
    ```python
    # arr: 초기 배열
    # 지원: point update (set value), range_sum(l, r) returns sum over [l, r) (0-index)

    def build_iterative(arr):
        n = len(arr)
		size = 1
		while size < n:
				size <<= 1
		tree = [0] * (2 * size)
		
		# build leaves
		for i in range(n):
				tree[size + i] = arr[i]
		# build internal nodes
		for i in range(size-1, 0, -1):
				tree[i] = tree[2*i] + tree[2*i+1]
		return tree, size

    def point_update(tree, size, pos, value):
            i = pos + size
            tree[i] = value
            i //= 2
            while i:
                    tree[i] = tree[2*i] + tree[2*i+1]
                    i //= 2

    def range_sum(tree, size, l, r):
            # sum over [l, r)
            l += size; r += size
            res = 0
            while l < r:
                    if l & 1:
                            res += tree[l]
                            l += 1
                    if r & 1:
                            r -= 1
                            res += tree[r]
                    l //= 2; r //= 2
            return res

    # 사용 예
    # arr = [1, 2, 3, 4, 5]
    # tree, size = build_iterative(arr)
    # print(range_sum(tree, size, 1, 4))
            # 9
    # point_update(tree, size, 2, 10)
    #print(range_sum(tree, size, 1, 4))
            # 16
    ```

2. 재귀 + Lazy Propagation - 구간 덧셈, 구간 합 질의
    ```python
    # 0-indexed, query/update on interval [ql, qr] (inclusive)
    import sys
    sys.setrecursionlimit(1 << 20)

    def build_recursive(arr):
            n = len(arr)
            size = 4*n
            tree = [0] * size
            lazy = [0] * size
            
            def build(node, l, r):
                    if l == r:
                            tree[node] = arr[l]
                            return
                    m = (l + r) // 2
                    build(node*2, l, m)
                    build(node*2+1, m+1, r)
                    tree[node] = tree[node*2] + tree[node*2+1]
            build(1, 0, n-1)
            return tree, lazy

    def push(node, l, r, tree, lazy):
            if lazy[node] != 0:
                    val = lazy[node]
                    tree[node] += val * (r - l + 1)
                    if l != r:
                            lazy[node*2] += val
                            lazy[node*2+1] += val
                    lazy[node] = 0

    def update_range(node, l, r, ql, qr, val, tree, lazy):
            push(node, l, r, tree, lazy)
            if qr < l or r < ql:
                    return
            if ql <= l and r <= qr:
                    lazy[node] += val
                    push(node, l, r, tree, lazy)
                    return
            m = (l + r) // 2
            update_range(node*2, l, m, ql, qr, val, tree, lazy)
            update_range(node*2, m+1, r, ql, qr, val, tree, lazy)
            tree[node] = tree[node*2] + tree[node*2+1]

    def query_range(node, l, r, ql, qr, tree, lazy):
            push(node, l, r, tree, lazy)
            if qr < l or r < ql:
                    return 0
            if ql <= l and r <= qr:
                    return tree[node]
            m = (l + r) // 2
            return query_range(node*2, l, m, ql, qr, tree, lazy) + query_range(node*2+1, m+1, r, ql, qr, tree, lazy)

    # 사용 예시
    # arr = [1, 2, 3, 4, 5]
    # tree, lazy = build_recursive(arr)
    # update_range(1, 0, len(arr)-1, 1, 3, 10, tree, lazy)
            # arr[1..3] += 10
    # print(query_range(1, 0, len(arr)-1, 2, 4, tree, lazy))
            # 합 구하기
    ```

## 활용 팁 및 주의 사항
- 문제에 맞는 모델 선택
    - 구간 합/점 갱신이면 Fenwick Tree(BIT)가 더 간단하고 빠름
    - 구간 갱신 또는 비교 등이 섞이면 Segment Tree가 더 적합
- 0-index vs 1-index, [l, r) vs [l, r]을 코드 전체에 일치시키기
- 재귀 깊이(Python)
    - 재귀 구현은 n이 큰 경우 스택 깊이 문제가 발생할 수 있음
    - `sys.setrecursionlimit`로 늘릴 수는 있지만, 안전을 위해 반복형(bottom-up) 구현을 권장
- 합 외의 다른 연산
    - 합이 아닌 경우(최솟값, 최댓값, gcd 등) 합을 계산하던 부분을 해당 연산으로 바꾸되 항등원을 올바르게 설정해야 함
- 성능
    - C++에서는 재귀형도 빠르지만 Python에서는 bottom-up이 훨씬 빠름

## 추가 정보
- Persistent Segment Tree
    - 시간에 따른 배열 변화를 버전 별로 보관하면서 과거 버전에도 질의 가능
    - 이분 탐색 + 버전 트리를 조합한 문제에 자주 쓰임
- Segment Tree Beats
    - 구간에서 특정 복잡한 갱신을 더 효율적으로 처리하는 고급 기법
- AtCoder Library의 segtree / lazysegtree
    - C++에서 일반적으로 검증된 템플릿으로 사용 가능
- 대체 기법
    - Mo's algorithm(오프라인 쿼리)
    - sqrt-decomposition
    - 문제 성격에 따라 더 적합한 방법이 존재할 수 있음