# Two-Pointers Alogorithm

## 개요
- 배열이나 문자열 등 연속된 자료 구조에서 두 개의 인덱스(포인터)를 적절히 이동시키며 원하는 조건을 만족하는 부분을 찾는 기법
- 단일 반복문으로 이중 반복문을 대신할 수 있어, 시간 복잡도를 획기적으로 줄이는 데 유용함

## 기본 아이디어
- 일반적으로 왼쪽 포인터와 오른쪽 포인터를 두고, 어떤 조건을 만족할 때까지 두 포인터를 이동시킴
- 주로 조건에 따라 한 쪽 포인터를 한 칸씩 이동시키며 탐색
- 두 포인터는 배열을 최대 한 번씩만 훑기 때문에 O(N) 내에 해결 가능

## 예시 코드
1. 정렬 배열에서 두 수의 합 찾기
    ```python
    def two_sum_sorted(arr, target):
        l, r = 0, len(arr) - 1
        while l < r:
            s = arr[l] + arr[r]
            if s == target:
                return l, r
            elif s < target:
                l += 1
            else:
                r -= 1
        return None
    ```

2. 가변 길이 슬라이딩 윈도우 : 최대 합 부분 배열
    ```python
    def max_subarray_sum(arr, k):
        window_sum = sum(arr[:k])
        max_sum = window_sum
        for i in range(k, len(arr)):
            window_sum += arr[i] - arr[i-k]
            if window_sum > max_sum:
                max_sum = window_sum
        return max_sum
    ```

3. 연결 리스트 사이클 감지 (빠른, 느린 포인터)
    ```python
    class ListNode:
        def __init__(self, x):
            self.val, self.next = x, None
    
    def has_cycle(head):
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow is fast:
                return True
            return False
    ```

## 활용 팁 및 주의사항
- 경계조건
    - 포인터 교차(l < r) 여부와 배열 크기가 0, 1인 경우 처리
- 정렬 전제 확인
    - 투 포인터 타입에 따라 사전 정렬이 필요할 수 있음
- 슬라이딩 윈도우 vs. 투 포인터
    - 부분 합처럼 구간 합을 유지할 땐 슬라이딩 윈도우가 더 적합

## 추가 정보
- 응용
    - 이분 탐색, 분할 정복과 결합해 **두 배열의 중앙값** 문제에도 적용 가능