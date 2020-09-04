# 조건절 표현
## np.where
np.where(조건, 참, 거짓)
``` python
tarr = np.array([1,1,1,1])
farr = np.array([0,0,0,0])
cond = np.array([True,False,True,True])

>>> np.where(cond, tarr, farr)
array([1,0,1,1])
```
``` python
arr = np.arange(8).reshape(2,4)

>>> np.where(arr > 3, arr, -1)
array([[-1, -1, -1, -1],
       [ 4,  5,  6,  7]])
```

# 수학, 통계 메서드
* 통계 수학
``` python
# sum
# mean
# std, var
# min, max
# argmin, argmax
# cumsum, cumprod

arr = np.arange(16).reshape(2,8)
>>> arr.mean() # np.mean(arr)
7.5

>>> arr.mean(axis=0) # 세로(column)요소들의 mean
array([ 4.,  5.,  6.,  7.,  8.,  9., 10., 11.])

>>> arr.mean(axis=1) # 가로(row)요소들의 mean
array([ 3.5, 11.5])
```

* 선형대수
``` python
# 행렬 곱 (dot product)
x.dot(y)
np.dot(x,y)
x @ y
```
``` python
import numpy.linalg

# diag : 정사각행렬 생성, 추출. 정사각행렬의 대각/비대각 원소를 배열로 반환/1차원을 배열을 대각요소로 정사각행렬 생성
# dot : 행렬 곱
# trace : 행렬의 대각 원소의 합
# det : 행렬식 (determinant)
# eig : 정사각 행렬의 고유값과 고유벡터 계산
# inv : 역행렬 계산
# pinv : 무어-펜로즈 우사역원 역행렬 계산
# qr : QR 분해 
# svd : 특잇값 분해(SVD) 계산
# solve : 정사각행렬 A의 Ax = b의 x(해)를 계산
# lstsq : Ax = b를 만족하는 최소제곱해를 계산
```

# 불리언 배열 메서드
``` python
arr = np.array([True,False,True,True])
# 불리언 배열의 True 개수 count
>>> (arr==True).sum()
3

# any : 하나 이상의 값이 True이면 참
>>> arr.any()
True

# all : 모든 값이 True이면 참
>>> arr.all()
False
```

# 집합
``` python
# unique(x)
# intersect1d(x,y) : x, y 공통 요소
# union1d(x,y) : x, y 합집합
# in1d(x,y) : x가 y의 부분집합인지 불리언 반환
# setdiff1d(x,y) : x, y의 차집합
# setxor1d(x,y) : x, y의 대칭차집합. (한 배열에는 모두 있지만 한 배열에는 모두 없는 원소 반환)
```

# 정렬
``` python
arr = np.array([[10,3,5],[-3,6,1]])

>>> arr.sort() # arr.sort(axis=1)
array([[ 3,  5, 10],
       [-3,  1,  6]])

>>> arr.sort(axis=0)
array([[-3,  3,  1],
       [10,  6,  5]])
```

# 난수
``` python
# seed 고정
np.random.seed(777)

# permutation : 임의의 순열 반환, 순서의 임의 변경
# shuffle : 리스트나 배열의 순서 뒤섞음
# rand : 균든분포 표본 추출
# randint : 주어진 범위의 임의의 정수 추출
# randn : 평균=0, 표준편차=1인 정규분포 표본 추출
# binomial : 이항분포 표본 추출
# normal : 정규분포(가우시안) 표본 추출
# beta : 베타분포 표본 추출
# chisquare : 카이제곱분포 표본 추출
# gamma : 감마분포 표본 추출
# uniform : [0,1)의 균등분포 추출
```



# 배열 데이터 파일 입출력
## save
``` python
np.save('save_array', arr)
```
## load
``` python
np.load('save_array.npy')
```
## savez, savez_compressed
``` python
np.savez('array_zip.npz', a=arr1, b=arr2)

arrzip = np.load('array_zip.npz')
>>> arrzip['a']
arr1
```