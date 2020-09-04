# sort, sorted
* sort : 새로운 배열을 생성하지 않고 직접 배열을 정렬한다.
* sorted : 새로운 정렬된 배열을 생성.

# 간접 정렬: argsort, lexsort
* argsort
``` python
values = np.array([5, 0, 1, 3, 2])
idx = values.argsort()

>>> idx
array([1, 2, 4, 3, 0])

>>> values[idx]
array([0, 1, 2, 3, 5])
```
``` python
arr = np.random.randn(3,5)
arr[0] = values

>>> arr[:,arr[0].arrsort()]
array([[      0,      1,      2,       3,        5],
       [-0.1378, 0.2636, 2.1777, -0.4573,   0.3847],
       [ 0.2034, 0.7933, 1.9475,  0.38492, -0.3943]])
```
* lexsort
``` python
first_name = np.array(['Bob', 'Jane', 'Steve', 'Bill', 'Barbara'])
last_name = np.array(['Jones', 'Arnold', 'Arnold', 'Jones', 'Walters'])
sorter = np.lexsort((first_name, last_name))

>>> sorter
array([1, 2, 3, 0, 4])

>>> zip(last_name[sorter], first_name[sorter])
<zip at 0x7fa203eda1c8>
```
나중에 넘겨준 배열이 데이터를 정렬하는데 먼저 사용.
## 대안 정렬 알고리즘
``` python
values = np.array(['2: first', '2:second', '1:first', '1: second', '1:third'])
key = np.array([2, 2, 1, 1, 1])
idx = key.argsort(kind='mergesort')

>>> idx
array([2, 3, 4, 0, 1])

>>> values.take(idx)
array(['1:first', '1: second', '1:third', '2: first', '2:second'], dtype='<u8')
```
# 배열 일부만 정렬
* partition / argpartition : K개의 작은 원소를 정렬
``` python
arr = np.array([7, 2, 3, 1, 5, 4, 6])

>>> np.partition(arr, 3) # 작은 3개 왼쪽에 정렬
array([2, 3, 1, 4, 5, 6, 7])

>>> np.partition(arr, -3) # 큰 3개 오른쪽에 정렬
array([2, 3, 1, 4, 5, 6, 7])

>>> np.argpartition(arr, 3)
array([1, 2, 3, 5, 4, 6, 0])
```
# searchsorted
이진 탐색을 수행해 새로운 값을 삽입할 때 정렬된 상태를 계속 유지하기 위한 위치를 반환
``` python
arr = np.array([0, 1, 7, 12, 15])

>>> arr.searchsorted(9)
3

>>> arr.searchsorted([0,8,11,16])
array([0, 3, 3, 5])
```
``` python
arr = np.array([0, 0, 0, 1, 1, 1])

>>> arr.searchsorted([0,1])
array([0, 3])

>>> arr.searchsorted([0,1], side='right')
array([3, 6])
```
* 활용 예제
``` python
data = np.floor(np.random.uniform(0, 10000, size=50))
bins = np.array([0, 100, 1000, 5000, 10000])
labels = bins.searchsorted(data)

>>> pd.Series(data).groupby(labels).mean()
2     656.000000
3    2898.136364
4    6723.285714
dtype: float64
```
