# 자료구조
## Series
series는 일련의 객체를 담을 수 있는 1차원 배열과 같은 자료구조이다. 그리고 `색인`이라는 데이터의 이름을 가진다. 
``` python 
import pandas as pd

obj = pd.Series([4,7,-5,3], index=['a','b','c','d'])
obj = pd.Series({'a':4,'b':7,'c':-5,'d':3})
>>> obj
a 4
b 7
c -5
d 3

>>> obj.index
Index(['a','b','c','d', dtype='object'])

>>> obj.values
array([4,7,-5,3])
```
``` python
>>> obj[['a','c']]
a 4
c -5

>>> obj['c'] = 0

>>> obj[obj>2]
a 4
b 7
d 3

>>> obj * 2
a 12
b 14
c 0
d 6

# name 속성
>>> obj.name = 'alpha data'
>>> obj.index.name = 'alpha'
alpha
a  4
b  7
c  -5
d  3
Name: alpha data, dtype: int64
```

## DataFrame

``` python
data = {'state':['Ohio','Ohio','Nevada','Nevada'],
        'year':[2000,2001,2001,2002],
        'pop':[1.5,1.7,2.4,2.9]}
df = pd.DataFrame(data)

# columns 순서 지정
df = pd.DataFrame(data, columns=['year','state','pop'])

# 중첩 dict를 이용
pop = {'Navada':{2001:2.4, 2002:2.9},
       'Ohio':{2000:1.5, 2001:1.7}}
df = pd.DataFrame(pop)
```
``` python
# column 접근
df['state']
df.state

# column 생성
df['eastern'] = [True, True, False, False]

# Transpose
df.T
```

## 색인 객체
pandas의 색인 객체는 표 형식의 데이터에서 각 row와 coloumn에 대한 이름과 다른 메타 데이터를 저장하는 객체이다.
``` python
pop = {'Navada':{2001:2.4, 2002:2.9},
       'Ohio':{2000:1.5, 2001:1.7}}
df = pd.DataFrame(pop, columns=['Ohio','Navada'])
df.columns.name = 'state'
df.index.name = 'year'

index = df.index
>>> index[:]
Int64Index([2000, 2001, 2002], dtype='int64', name='year')

>>> index[1] = '2003' #색인 객체는 변경 불가!

>>> df.columns
Index(['Ohio', 'Navada'], dtype='object', name='state')
```
* 색인 메서드
``` python
# append : 색인 객체 추가
# difference : 색인의 차집합
# intersection : 색인의 교집합
# union : 색인의 합집합
# isin : 색인의 존재여부
# delete : 위치 인자의 색인 삭제
# drop : 값 인자의 색인 삭제
# insert : 위치 인자에 새로운 색인 추가
# is_monotonic : 색인이 단조성 여부
# is_unique : 색인의 중복 여부
# unique : 중복 요소를 제거한 색인 객체 반환
```

# 핵심기능
## 재색인 : reindex
* Series
``` python
obj = pd.Series([1,2,3,4], index=['a','b','c','d'])
obj2 = obj.reindex(['b','c','d','e'])
>>> obj2
b 2
c 3
d 4
e NaN

# ffill / bfill
obj = pd.Series(['a','b','c'], index=[0,2,4])
>>> obj.reindex(range(6), method='ffill')
0 a
1 a
2 b
3 b
4 c
5 c

>>> obj.reindex(range(6), method='bfill')
0 a
1 b
2 b
3 c
4 c
5 NaN
```
* DataFrame
``` python
df = pd.DataFrame(np.arange(9).reshape(3,3),
                 index=['a','b','c'], columns=['X','Y','Z'])

>>> df.reindex(['a','c','d'])
   X	  Y	 Z
a	0.0	1.0	2.0
c	6.0	7.0	8.0
d	NaN	NaN	NaN

>>>df.reindex(columns=['X','Z','R'])
  X	Z  R 
a	0	2	NaN
b	3	5	NaN
c	6	8	NaN
```
* reindex 함수 인자
``` python
# index/columns : 새로운 인덱스 값
# method : 채움 매서드. ffill/bfill
# fill_value : NaN을 대체할 값
# limit : 전/후 보간 시에 사용할 최대 갭 크기 (채워넣을 원소의 수)
# tolerance : 전/후 보간 시에 사용할 최대 갭 크기 (값의 차이)
# level : Multi-index의 level에 색인을 맞춤
# copy : 이전 색인과 동일할 때 복사 여부. True=동일해도 데이터 복사. False=동일하면 복사 안함.
```
## row, column 삭제 : drop
* Series
``` python
obj = pd.Series([1,2,3,4], index=['a','b','c','d'])

>>> obj.drop('c')
a 1
b 2
d 4
```
* DataFrame
``` python
df = pd.DataFrame(np.arange(9).reshape(3,3),
                 index=['a','b','c'], columns=['X','Y','Z'])

>>> df.drop(['b','c'])
	X	Y	Z
a	0	1	2

>>> df.drop('X', axis=1)
>>> df.drop('X', axis='columns')
	Y	Z
a	1	2
b	4	5
c	7	8
```
## 색인, 선택 : loc, iloc
* Series
``` python
obj = pd.Series([10,11,12,13], index=['a','b','c','d'])

>>> obj['c']
12
>>> obj[1]
11
>>> obj[2:4]
c 12
d 13
>>> obj[2:4] = 20
>>> obj
a 10
b 11
c 20
d 20
```
* DataFrame
``` python
df = pd.DataFrame(np.arange(9).reshape(3,3),
                 index=['a','b','c'], columns=['X','Y','Z'])

>>> df[['X','Y']]
  X	Y
a	0	1
b	3	4
c	6	7
>>> df[df['X']>3]
X	Y	Z
c	6	7	8

# loc , iloc
>>> df.loc[['a','c'],['X','Z']]
X	Z
a	0	2
c	6	8

>>> df.iloc[[0,2],[0,2]]
X	Z
a	0	2
c	6	8
```
색인은 색인의 중복 여부에 따라 반환 type이 달라진다. 색인의 중복 여부에 따라 때로는 코드가 복잡해 질 수 있으니 주의하도록 하자. `is_unique` 속성을 이용해 중복 여부를 확인할 수 있다.
## 산술 연산
pandas 객체 간 산술 연산은 데이터베이스의 외부 조인과 유사하다.
공통된 색인에 대해서 연산을 수행하고 그렇지 않은 색인들은 NaN으로 표시된다.
* Series
``` python
s1 = pd.Series([1,2,3], index=['a','b','c'])
s2 = pd.Series([4,5,6], index=['a','c','d'])

>>> s1 + s2
a 5
b NaN
c 8
d NaN
```
* DataFrame
``` python
df1 = pd.DataFrame(np.arange(9).reshape(3,3), columns=list('bcd'),
                  index=['Ohio','Texas','Colorado'])
df2 = pd.DataFrame(np.arange(12).reshape(4,3), columns=list('bde'),
                  index=['Utah','Ohio','Texas','Oregon'])

>>> df1 + df2
	         b	 c	 d	  e
Colorado	NaN	NaN	NaN	 NaN
Ohio	    3.0	NaN	6.0	 NaN
Oregon	  NaN	NaN	NaN	 NaN
Texas	    9.0	NaN	12.0 NaN
Utah	    NaN	NaN	NaN	 NaN

# 연산 매서드와 값 채워 넣기 : fill_value
>>> df1.add(df2, fill_value=0)
           b	 c	 d	  e
Colorado	6.0	7.0	8.0	 NaN
Ohio	    3.0	1.0	6.0	 5.0
Oregon	  9.0	NaN	10.0 11.0
Texas	    9.0	4.0	12.0 8.0
Utah	    0.0	NaN	1.0	 2.0
```
* DataFrame - Series
``` python
df = pd.DataFrame(np.arange(12).reshape(4,3), columns=list('bde'),
                  index=['Utah','Ohio','Texas','Oregon'])

>>> df - df.loc['Utah'] # Series의 색인을 columns에 맞추고 row로 전파
       b	d	e
Utah	 0	0	0
Ohio	 3	3	3
Texas	 6	6	6
Oregon 9	9	9

>>> df.sub(df.loc['d'], axis='index')
        b d	e
Utah	 -1	0	1
Ohio	 -1	0	1
Texas	 -1	0	1
Oregon -1	0	1
```
## 함수 적용과 매핑
* map : map 함수 특성상 Series에 적용가능
``` python
s1 = pd.Series(np.random.randn(4))
>>> s1.map(lambda x: '%.2f' %x)
0    -0.75
1    -2.01
2    -0.18
3     0.38
```
* apply / applymap
``` python
df = pd.DataFrame(np.random.randn(4,3), columns=list('bde'),
                 index=['Utah','Ohio','Texas','Oregon'])
>>> np.abs(df) # ufunc

>>> df.apply(lambda x: x.max() - x.min())
b 1.802
d 1.684
e 2.689

>>> df.apply(lambda x: x.max() - x.min(), axis='columns')
Utah  0.988
Ohio  2.521
Texas 0.678
Oregon 2.431

>>> df.applymap(lambda x: '%.2f' %x)
# applymap은 개별 요소에 함수를 적용한다.
```

## 정렬 : sort_index , sort_values
* Series
``` python
obj = pd.Series(range(4), index=list('dcba'))

>>> obj.sort_index()
a    3
b    2
c    1
d    0

obj2 = pd.Series([4,9,np.NaN, 10,3])
>>> obj2.sort_values()
4     3.0
0     4.0
1     9.0
3    10.0
2     NaN
```
* DataFrame
``` python
df = pd.DataFrame(np.random.randn(4,3), columns=list('bde'),
                 index=['Utah','Ohio','Texas','Oregon'])

>>> df.sort_index()
>>> df.sort_index(axis=1, ascending=False)

df2 = pd.DataFrame({'b':[4,7,-3,2], 'a':[0,1,0,1]})
>>> df2.sort_values(by='b', ascending=False)
  	a	b
1 	1	7
0 	0	4
3 	1	2
2 	0	-3

>>> df2.sort_values(by=['a','b'])
   a	 b
2	 0	-3
0	 0	4
3	 1	2
1	 1	7
```
## 순위 : rank
* Series
``` python
obj.rank()
# 데이터 상 순서에 따라 
obj.rank(method='first')
```
* DataFrame
``` python
df.rank(axis='columns')
```

# 통계와 요약
## 요약 통계

pandas의 통계 메서드는 기본적 누락값을 제외하도록 설계되었다.
``` python
df = pd.DataFrame(np.random.randn(4,3), columns=list('bde'),
                 index=['Utah','Ohio','Texas','Oregon'])

>>> df.info()
"""
<class 'pandas.core.frame.DataFrame'>
Index: 4 entries, Utah to Oregon
Data columns (total 3 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   b       4 non-null      float64
 1   d       4 non-null      float64
 2   e       4 non-null      float64
 """

>>> df.describe() # 요약 통계 계산
          b	         d	         e
count	4.000000	 4.000000	   4.000000
mean	0.645432	 -0.193046	-0.455094
std	  1.266286	 0.289345	   1.000425
min	  -0.808346	 -0.614319	-1.704203
25%	  0.088270	 -0.258742	-0.845544
50%	  0.562751	 -0.093961	-0.422756
75%	  1.119913	 -0.028265	-0.032306
max	  2.264574	  0.030057	 0.729339
```
축소 메서드는 기본적으로 열에 대하여 계산한다. 이는 axis로 조정할 수 있다. 또한 skipna 옵션을 통해 누락값에 대한 옵션을 줄 수 있다.
``` python
>>> df.sum()
b   -1.475447
d    1.015088
e    0.096531

>>> df.sum(axis='columns', skipna=False)
Utah     -3.587424
Ohio     -1.633097
Texas    -0.731774
Oregon    0.619068
```
* 요약 통계 메서드
``` python
# count
# min, max
# argmin, argmax
# idxmin, idxmax
# quantile
# sum, mean, median, mad
# prod
# var, std, skew, kurt
# cumsum, cummin, cummax, cumprod
# diff, pct_change
```
## 상관관계와 공분산
``` python
df.corr() # 상관관계
df.corrwith(df2) # 서로 다른 객체와의 상관관계

df.cov() # 공분산
```
## unique, value_counts, isin

* 유일값 : unique
``` python
# 중복된 값을 제거하고 유일값만 반환
obj.unique()
```
* 값 세기 : value_counts
``` python
obj.value_counts() # 도수 계산, 내립차순 정렬
pd.value_counts(obj.values, sort=False)
```
* 멤버십 : isin
``` python
# 어떠한 값이 존재하는지 여부 반환
obj.isin(['a','b','c'...])
```