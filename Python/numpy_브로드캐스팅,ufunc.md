# 브로드 캐스팅
`브로드 캐스팅`은 다른 모양의 배열 간의 산술 연산을 가능하게 해준다.
``` python
arr = np.random.randn(2,3)

>>> arr.mean(0)
array([-0.3928, -0.3824, -0.8768])

>>> arr - arr.mean(0)
array([[0.3937, 1.7263, 0.1633],
       [0.4384, 1.9878, 0.9873]])

>>> arr - arr.mean(1).reshape(2,1)
array([[0.3784, 0.4912, 0.9853],
       [0.5963, 0.1821, 0.5869]])
```
* 브로드 캐스팅을 이용한 값 대입
``` python
arr = np.zeros((2,3))

>>> arr[:] = 5
array([[5,5,5],
       [5,5,5]])

col = np.array([1.28, 0.42, 0.44, 1.65])

>>> arr[:] = col[:, np.newaxis]
array([[1.28, 1.28, 1.28],
       [0.42, 0.42, 0.42],
       [0.44, 0.44, 0.44],
       [1.65, 1.65, 1.65]])
```

# 고급 ufunc
reduce는 하나의 배열을 받아서 순차적인 이항 연산을 통해 축에 따라 그 값을 집계해준다.
``` python
arr = np.arange(10)

>>> np.add.reduce(arr)
45

>>> arr.sum()
45
```
``` python
arr = np.arange(15).reshape((3,5))

>>> np.add.accumulate(arr, axis=1)
array([[ 0,  1,  3,  6, 10],
       [ 5, 11, 18, 26, 35],
       [10, 21, 33, 46, 60]])
```
* ufunc 메서드
``` python
reduce(x) : 연산의 연속된 적용으로 값을 집계
accumulate(x) : 모든 부분적 집계값을 우지한 채 값을 집계
reduceat(x, bins) : 로컬 reduce 또는 groupby. 연속된 데이터 슬라이스를 집계된 배열로 축소
outer(x,y) : x, y의 모든 원소 조합에 대한 연산을 적용.
```

# 구조화된 배열
``` python
dtype = [('x', np.float64),('y', np.int32)]
arr = np.array([(1.5, 6), (np.pi, -2)], dtype=dtype)

>>> arr
array([(1.5, 6), (3.1416, -2)], dtype=[('x','<i8'), ('y','<i4')])

>>> arr[0]
(1.5, 6)

>>> arr['x']
array([1.5, 3.1416])
```