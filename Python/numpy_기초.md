# Numpy

Numpy는 Numerical Python의 줄임말로, 산술 계산을 위한 필수 패키지이다.
배열 연산을 기반으로 대량의 데이터도 빠르고 가볍게 처리할 수 있다는 장점이 있다.

## ndarray : 다차원 배열
* 배열 생성, 배열 형태
``` python
import numpy as np

# array 생성
list2arr = [1,2,3,4]
arr = np.array(list2arr, dtype=np.float32)
>>> arr
array([1., 2., 3., 4.])

list2arr_2 = [[1,2,3,4],[5,6,7,8]]
arr2 = np.array(list2arr_2)
>>> arr2
array([[1,2,3,4],
      [5,6,7,8]])
# array 차원
>>> arr2.ndim
2
# array shape
>>> arr2.shape
(2,4)
# array dtype
>>> arr2.dtype
dtype('int64')
```
* 기타 배열 생성 함수
``` python
# asarray : array 생성. 이미 array인 경우 객체를 복사하지 않음

# arange : array형태의 range 반환
np.arange((10))
np.arange((3,6))
# ones, ones_like : 1로 초기화된 배열 생성. 
>>> np.ones(5)
array([1,1,1,1,1])
>>> np.ones(5) * 2
array([2,2,2,2,2])

>>> np.ones_like(arr)
array([1,1,1,1])

# zeros, zeros_like : 0으로 초기화된 배열 생성
# eye, identity : 대각행렬이 1인 단위행렬 생성
# empty, empty_like : 초기화되지 않은 배열 생성
# full, full_like : 인자 값으로 채운 배열 생성
```
* 데이터 타입 변환 : astype
``` python
arr_int = arr.astype(np.int64)
>>> arr_int
array([1,2,3,4])

# string_
arr_str = arr.astype(np.string_)
>>> arr_str
array([1,2,3,4])
```

## 배열의 연산
배열 연산의 핵심은 **벡터화**이다. 이는 같은 크기의 배열 연산에서 각 원소 단위로 연산을 해준다.
``` python
arr = np.array([[1,2,3],[4,5,6]])

# 산술연산 : + , - , * , / , ** 등 산술연산 가능
arr2 = arr * arr
>>> arr2
array([1,4,9],[16,25,36])

# 불리언 연산
>>> arr2 > arr
array([False,True,True],[True,True,True])
```
크기가 다른 배열간 연산은 브로드캐스팅 된다.

## 색인과 슬라이싱
``` python
arr = np.array([1,2,3,4,5])

>>> arr[2:4]
array([3,4])

>>> arr[2:4] = 10
>>> arr
array([1,2,10,10,5])
```
numpy array의 색인은 해당 객체를 복사하지 않고 곧 바로 참조(뷰)한다. 만약 해당 객체를 복사하고 싶다면 `copy`를 활용하자.

* 다차원 배열의 색인
``` python
arr = np.array([[1,2,3],[4,5,6]])

>>> arr[0][1]
2
>>> arr[0,1]
2
>>> arr[:,1]
array([2,5])
```
* 불리언 색인
``` python
alp_idx = ['A','B','A','C']
data = np.array([[1,2,3],[4,5,6],[7,8,9],[10,11,12]])
idx = alp_idx == 'A'

>> data[idx,1:]
array([[2,3],[8,9]])

# != 연산자 : ~
>>> data[~idx,:]
array([4,5,6])

# and , or : & , | - numpy에서는 and, or 사용 불가.
idx2 = (alp_idx=='A')|(alp_idx=='C')
>>> data[idx2]
array([[1,2,3],[7,8,9],[10,11,12]])
```
* 팬시 색인 (정수 색인)
``` python
arr = np.array([[1,2,3],[4,5,6],[7,8,9],[10,11,12]])

>>> arr[[3,2,1]]
array([[10,11,12],[7,8,9],[4,5,6]])

>>> arr[[0,2],[-1,1]] # (0,2), (-1,1)
[3, 11]

>>> arr[[0,-1]][:,[1:]]
array([[2,3], [11,12]])
```
## 전치와 축 바꾸기
* Transpose
``` python
arr = np.array([[1,2,3],
                [4,5,6],
                [7,8,9],
                [10,11,12]])

>>> arr.T
array([[1,4,7,10],
       [2,5,8,11],
       [3,6,9,12]])
```
``` python
arr = np.arange(16).reshape((2,2,4))
>>> arr
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7]],
       [[ 8,  9, 10, 11],
        [12, 13, 14, 15]]])

>>> arr.transpose((1,0,2))
array([[[ 0,  1,  2,  3],
        [ 8,  9, 10, 11]],
       [[ 4,  5,  6,  7],
        [12, 13, 14, 15]]])
```
* swapaxes
swapaxes도 데이터를 복사하지 않고 뷰를 반환한다.
``` python
>>> arr.swapaxes(1,2)
array([[[ 0,  4],
        [ 1,  5],
        [ 2,  6],
        [ 3,  7]],
       [[ 8, 12],
        [ 9, 13],
        [10, 14],
        [11, 15]]])
```

## 유니버셜 함수 : ufunc
유니버셜 함수는 ndarray 안에 있는 원소별 연산을 수행한다.
* 단항 유니버셜 함수
``` python
arr = np.arange(5)

>>> np.square(arr)
array([0, 1, 4, 9, 16, 25])

# abs, fabs : 절대값 함수
# sqrt : 제곱근 함수
# square : 제곱 함수
# exp : 지수 e 함수
# log, log10, log2, log1p : 로그함수_ln, log_10, log_2, log(1+p)
# sign : 부호계산
# ceil : 크거나 같은 정수 반환
# floor : 작거나 같은 정수 내림
# rint : 가까운 정수로 반올림
# modf : 몫과 나머지 반환
# isnan : 숫자 여부 반환
# isfinite, isinf : 유한 여부 반환
# cos, cosh, sin, sinh, tan, tanh : 삼각함수 쌍곡삼각함수
# arccos, acrcosh, arcsin, arcsinh, arctan, arctanh : 역삼각함수
# logical_not : 논리 부정값 계산
```
* 이항 유니버셜 함수
``` python
x = np.random.randn(3)
y = np.random.randn(3)

>>> np.add(x,y)

# add : 배열 원소간 합
# subtract : 배열 원소간 차
# multiply : 배열 원소간 곱
# divide, floor_divide : 배열 원소간 나누기. floor_divide는 몫만 반환
# power : x^y
# maximun, fmax : 배열 원소간 최대값 반환. fmax는 Nan 무시
# minimun, fmin : 배열 원소간 최소값 반환. fmin은 Nan 무시
# mod : x%y
# copysign : x, y의 원소간 기호 swap
# greater, greater_equal, less, less_equal, equal, not_equal : 비교연산자
# logical_and, logical_or, logical_xor : &, |, ^ 연산
```

# cf) copy
- 얕은 복사
``` python
import copy

a = [1,2,3]
b = a[:]
>>> a == b
True
>>> a is b
False
>>> b[0] = 5
[5, 2, 3]
```
리스트를 슬라이싱 하면 두 객체는 같은 요소를 갖지만 서로 다른 메모리에 할당된다.
하지만 이중 리스트의 경우 얘기가 조금 달라진다.
``` python
a = [[1,2],[3,4]]
b = a[:]
>>> a == b
True
>>> a is b
False

>>> a[0] is b[0]
True
```
중첩 리스트의 경우 각 객체는 서로 다른 객체이지만 그 내부의 요소는 같은 메모리를 공유한다.
``` python
a[0] = [7,8]
>>> a == b
False
>>> a is b
False

a[1].append(5)
>>> a
[[7,8],[3,4,5]]
>>> b
[[1,2],[3,4,5]]
```
이때 위와 같이 새로운 값을 할당하면 문제가 되지 않지만 a의 값을 변경하면 b도 영향을 받는다.
copy.copy()도 이와 같은 얕은 복사에 해당한다.

- 깊은 복사
깊은 복사는 동일한 객체를 새로운 메모리에 할당한다.
``` python
a = [[1,2],[3,4]]
b = copy.deepcopy(a)

a[1].append(5)
>>> a
[[1,2],[3,4,5]]
>>> b
[[1,2],[3,4]]
```
