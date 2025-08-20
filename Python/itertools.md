# itertools

## permutations

>파이썬 표준 라이브러리 `itertools` 모듈에 정의된 함수. 주어진 iterable에서 **순열**을 생성함.
>모든 순열을 C로 구현된 내부 루프에서 빠르게 생성하므로, 순열을 직접 재귀 호출로 구현하는 것보다 훨씬 효율적임
```python
itertools.permutations(iterable, r=None)
```
- iterable: 순열을 구할 원본 시퀀스
- r(선택): 순열의 길이. 지정하지 않으면 `len(iterable)`인 전체 길이의 순열을 생성
- 리턴값은 iterator이고, 각 요소는 길이 `r`의 튜플임


동작 방식 및 시간, 공간 복잡도
 - 
 - 기본적으로 n!개의 순열을 생성
 - `r<n`이라면 길이가 r인 순열을 생성
 - 시간 복잡도: O(nPr) 순으로, 순열 하나당 생성 비용은 상수에 가까움
 - 공간 복잡도: 출력하는 튜플의 크기뿐, 내부에서 추가적인 리스트 복사 등을 최소화함

사용 예시
 -
1. 전체 순열 생성
    ```python
    from itertools import permutations

    for order in permutations([1, 2, 3]):
        print(order)

    # 출력:
    # (1, 2, 3)
    # (1, 3, 2)
    # (2, 1, 3)
    # (2, 3, 1)
    # (3, 1, 2)
    # (3, 2, 1)
    ```

2. 부분 순열 생성
    ```python
    from itertools import permutations

    for p in permutaions('ABC', 2)
        print(''.join(p))
    # 출력:
    # AB
    # AC
    # BA
    # BC
    # CA
    # CB
    ```