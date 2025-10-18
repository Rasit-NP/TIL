# LIS(Longest Increasing Subsequence)

## 개요
- 주어진 수열에서 인덱스가 증가하는 원소들 중 값이 엄격히 증가하는 부분 수열 중 길이가 최대인 경우를 찾는 문제

## 기본 아이디어
1. Dynamic Programming
    - dp[i]를 i번째 원소까지 정의된 수열의 LIS로 정의, 초기값으로 0을 저장
    - i = 0부터 마지막까지 탐색하면서, lst[j] > lst[i] 와 j > i를 동시에 만족하는 j가 존재할 경우, dp[j]+=1
    - 최종 답은 max(dp)
    - 장점
        - 직관적이고 부분 수열을 바로 복원하기 쉬움
        - 단순한 구현 방버
    - 단점
        - 높은 시간 복잡도
2. Binary Search
    - 길이가 k인 증가 부분 수열들의 끝 값의 가능한 최솟값들을 tails 배열에 저장
    - 각 원소 x에 대해 tails에서 x가 들어갈 위치를 찾아 대체 혹은 맨 뒤에 추가
    - LIS의 길이는 len(tails)와 같음
        - tails의 원소는 실제 LIS와 다를 수 있음
    - 복원
        - 실제 부분 수열을 복원하려면, 각 원소의 부모 인덱스와 tails가 가리키는 원소들의 인덱스 배열을 함께 저장해야 함
    - 장점
        - 매우 빠름: O(N logN)

## 예시 코드
- Binary Search
    ```python
    from bisect import bisect_left


    # LIS의 길이 구하기
    def lis_len(arr):
            n = len(arr)
            if n == 0:
                    return 0
            
            tails = []
            tails_idx = []
            
            for i, x in enumerate(arr):
                    pos = bisect_left(tails, x)
                    if pos == len(tails):
                            tails.append(x)
                            tails_idx.append(i)
                    else:
                            tails[pos] = x
                            tails_idx[pos] = i

            length = len(tails)
            
            return length
            
    # LIS의 길이와 수열 복원하기
    def lis_with_reconstruct(arr):
            n = len(arr)
            if n == 0:
                    return 0, []
            
            tails = []
            tails_idx = []
            parent = [-1] * n
            
            for i, x in enumerate(arr):
                    pos = bisect_left(tails, x)
                    if pos == len(tails):
                            tails.append(x)
                            tails_idx.append(i)
                    else:
                            tails[pos] = x
                            tails_idx[pos] = i
                    
                    if pos > 0:
                            parent[i] = tails_idx[pos-1]
            
            lis = []
            k = tails_idx[-1]
            while k != 1:
                    lis.append(arr[k])
                    k = parent[k]
            lis.reverse()
            
            return len(lis), lis
    ```

## 활용 팁 및 주의 사항
- 엄격한 증가 vs 비감소
- 복원시 주의점
    - tails 값만 저장한 경우 복원이 불가능함
    - 인덱스와 부모를 함께 저장할 것
- 문제 풀이 팁
    - 문제에서 '부분 수열'이 아니라 '부분 배열'을 묻는다면 다른 알고리즘이 필요
    - 인덱스 기반 문제가 섞이면 복원 가능한 인덱스 배열을 요구할 수 있음

## 추가 정보
- 관련 개념
    - patience sorting
    - Longest Common Subsequence
    - Longest Non-Decreasing Subsequence
- 더 배우면 좋은 것
    - 이분 탐색의 lower_bound/upper_bound 개념과 구현 원리
    - 세그먼트 트리/팬윅 트리를 이용한 변형
    - 이진 인덱스 트리로의 변형 문제