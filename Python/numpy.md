# Numpy
```shell
pip install numpy
```
```python
import numpy as np
```
### Numpy
- `Numpy`(Numerical Python)는 수치 계산용 핵심 라이브러리로, **다차원 배열**을 기반으로 **백터화 연산**과 **수학, 통계 처리**를 빠르게 수행할 수 있음
- 데이터 분석, 인공지능, 통계 처리, 시뮬레이션 등에서 필수적으로 사용됨

### 배열 생성과 구조
```python
arr = np.array([[1, 2, 3],
                [4, 5, 6]])
print(arr.shape)        # (2, 3)
print(arr.ndim)         # 2
print(arr.dtype)        # int64 (또는 시스템에 따라 다름)
```
- 데이터 분석에서는 배열의 `shape`, `dtype`, `ndim`을 항상 확인해야 함
- 예측 모델 입력, 데이터 전처리 시 필수

### 주요 배열 생성 함수
```python
np.zeros((3, 3))        # 0으로 채운 배열
np.ones((2, 4))         # 1로 채운 배열
np.eye(4)               # 4x4 단위 행렬
np.arange(0, 10, 2)     # 0부터 10 전까지 2 씩
np.linspace(0, 1, 5)    # 0~1 tkdl 균등 분할 5개
np.random.randn(3, 3)   # 정규분포 난수
```

### 인덱싱 & 슬라이싱
```python
data = np.array([[10, 20, 30],
                [40, 50, 60],
                [70, 80, 90]])

print(data[0, 1])       # 20
print(data[:, 1])       # 열 단위: [20 50 80]
print(data[1:, :2])     # 행 단위: [[40, 50], [70, 80]]
```

### Broadcasting과 벡터화 연산
```python
arr = np.array([10, 20, 30])
print(arr + 5)          # [15, 25, 35]
print(arr * 2)          # [20, 40, 60]

matrix = np.array([[1, 2], [3, 4]])
print(matrix + np.array([10, 20]))  #Broadcasting
```

### 데이터 분석용 기본 통계 함수
```python
data = np.array([12, 15, 18, 20, 22, 25, 30])

np.mean(data)                       # 평균
np.median(data)                     # 중앙값
np.std(data)                        # 표준편차
np.var(data)                        # 분산
np.min(data)                        # 최소
np.max(data)                        # 최대
np.percentile(data, [25, 50, 75])   # 사분위수
```

### 축(axis)을 이용한 연산
```python
scores = np.array([[80, 90, 70],
                    [60, 85, 75],
                    [90, 95, 100]])

# 열 기준 평균
np.mean(scores, axis=0)     # [76.7, 90.0, 81.7]

# 행 기준 평균
np.mean(scores, axis=1)     # [80.0, 73.3, 95.0]
```

### 결측치(`NaN`) 처리
```python
data = np.array([10, 20, 30, 40, 50])

# 0~1 정규화
normalized = (data - np.min(data)) / (np.max(data) - np.min(data))

# 평균 0, 표준편차 1로 정규화
standardized = (data - np.mean(data)) / np.std(data)
```

### Boolean 인덱싱과 필터링
```python
data = np.array([10, 20, 30, 40, 50])

print(data[data > 25])      # [30, 40, 50]
print(data[(data > 20) & (data < 45)])
                            # [30, 40]
```

### `Numpy`와 `Pandas`의 연결
```python
import pandas as pd

arr = np.array([[1, 2, 3],
                [4, 5, 6],
                [7, 8, 9]])

df = pd.DataFrame(arr, columns=['A', 'B', 'C'])
df.to_numpy()           # 다시 Numpy 배열로 변환
```
- Pandas의 `DataFrame`은 내부적으로 `numpy.ndarray`를 기반으로 함

### 선형대수
```python
A = np.array([[1, 2],
            [3, 4]])

B = np.array([[2, 0],
            [1, 2]])

np.dot(A, B)        # 행렬 곱
np.linalg.inv(A)    # 역행렬
np.linalg.eig(A)    # 고유값과 고유벡터
```