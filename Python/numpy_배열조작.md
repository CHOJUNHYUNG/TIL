# 배열 조작
## reshape
``` python
arr = np.arange(8)

>>> arr.reshape((4,2))
array([[0,1],
       [2,3],
       [4,5],
       [6,7]])

>>> arr. reshape((-1,4))
array([[0,1,2,3],
       [4,5,6,7]])
```
### 평탄화: ravel / flatten
``` python
arr = np.arange(15).reshape((5,3))

>>> arr.reavel() # 복사본을 반환하지 않음.
array([0,1,2,3,4,5,6,7,8,9,10,11,12,13,14])

>>> arr.flatten() # 복사본을 반환함.
array([0,1,2,3,4,5,6,7,8,9,10,11,12,13,14])
```
## C 순서와 포트란 순서
* C : 로우 우선 순서 - 배열의 각 로우에 해당하는 데이터 공간이 인접 메모리에 적재
* 포트란 : 컬럼 우선 순서 -  배열의 각 칼럼에 해당하는 데이터 공간이 인접 메모리에 적재
``` python
arr = np.arange(12).reshape((3,4))

>>> arr.ravel()
array([0,1,2,3,4,5,6,7,8,9,10,11])

>>> arr.ravel('F')
array([0,4,8,1,5,9,2,6,10,3,7,11])
```
## concatenate
``` python
arr1 = np.array([[1,2,3],[4,5,6]])
arr2 = np.array([[7,8,9],[10,11,12]])

>>> np.concatenate([arr1,arr2], axis=0) # np.vstack((arr1,arr2)) , row_stack
array([[1,2,3],
       [4,5,6],
       [7,8,9],
       [10,11,12]])

>>> np.concatenate([arr1,arr2], axis=1) # np.hstack((arr1,arr2)) , column_stack
array([[1,2,3,7,8,9],
       [4,5,6,10,11,12]])
```
## split
``` python
arr = np.arange(10).reshape((5,2))

first, second, third = np.split(arr, [1,3]) # [1,3] = 분할 위치
>>> first
array([[0,1]])
>>> second
array([[2,3],
       [4,5]])
>>> third
array([[6,7],
       [8,9]])
```
## `r_`과 `c_`
``` python
arr = np.arange(6)
arr1 = arr.reshape((3,2))
arr2 = np.random.randn(3,2)

>>> np.r_[arr1,arr2]
array([[0. , 1.]
       [2. , 3.],
       [4. , 5.],
       [1.0072, 1.2962],
       [0.275, 0.228],
       [1.3529, 0.994]])

>>> np.c_[r_[arr1,arr2], arr]
array([[0.     , 1.     , 0.]
       [2.     , 3.     , 1.],
       [4.     , 5.     , 2.],
       [1.0072 , 1.2962 , 3.],
       [0.275  , 0.228  , 4.],
       [1.3529 , 0.994  , 5.]])

>>> np.c_[1:6, -10:-5]
array([[1 , -10],
       [2 , -9],
       [3 , -8],
       [4 , -7],
       [5 , -6]])
```
* 기타 이어 붙이기 함수
``` python
dstack : 깊이(axis=2)에 따라 stack
hsplit, vsplit : 각 axis=0, axis=1을 따라 분할
```
## repeat 과 tile
* repeat : 한 배열의 각 원소를 복제한 배열 생성
``` python
arr = np.arange(3)

>>> arr.repeat(3)
array([0,0,0,1,1,1,2,2,2])

>>> arr.repeat([2,3,4])
array([0,0,1,1,1,2,2,2,2])

# 다차원 배열의 경우 축을 따라 복제. 축을 지정하지 않으면 평탄화됨
arr = np.random.randn(2,2)
>>> arr.repeat(2, axis=0)
array([[-2.0014, -0.3718],
       [-2.0014, -0.3718],
       [1.6995 , -0.4395],
       [1.6995 , -0.4395]])
```
* tile : 축을 따라 배열을 복제하여 배열 생성
``` python
arr = np.arange(4).reshape((2,2))

>>> np.tile(arr, 2)
array([[0, 1, 0, 1],
       [2, 3, 2, 3]])

>>> np.tile(arr, (2,1))
array([[0, 1],
       [2, 3],
       [0, 1],
       [2, 3]])
```
## 팬시 색인: take와 put
``` python
arr = np.arange(10)*100
idx = [7,1,2,6]

>>> arr[idx] # arr.take(idx)
array([700, 100, 200, 600])

>>> arr.put(idx, [40, 41, 42, 43])
array([0, 41, 42, 300, 400, 500, 43, 40, 800, 900])
```
``` python
arr = np.arange(8).reshape(2,4)
idx = [2,0,2,1]

>>> arr.take(idx, axis=1)
array([[2, 0, 2, 1],
       [6, 4, 6, 5]])
```
