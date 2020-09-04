# 데이터 합치기
## merge
``` python
df_left = pd.DataFrame({'KEY': ['K0', 'K1', 'K2', 'K3'],
    'A': ['A0', 'A1', 'A2', 'A3'],
    'B': ['B0', 'B1', 'B2', 'B3']})

df_right = pd.DataFrame({'KEY': ['K2', 'K3', 'K4', 'K5'],
    'C': ['C2', 'C3', 'C4', 'C5'],
    'D': ['D2', 'D3', 'D4', 'D5']})

>>> pd.merge(df_left, df_right, on='KEY')
 KEY	A	  B	  C	  D
0	K2	A2	B2	C2	D2
1	K3	A3	B3	C3	D3
```
* on / left_on, right_on
merge는 기본적으로 공통된 column이름으로 병합한다. 하지만 on 옵션을 통해 명시적으로 지정하는 것이 좋다. 또한 중복된 column이 없는 경우 따로 지정할 수도 있다.
``` python
df2_left = pd.DataFrame({'lKEY': ['K0', 'K1', 'K2', 'K3'],
    'A': ['A0', 'A1', 'A2', 'A3'],
    'B': ['B0', 'B1', 'B2', 'B3']})

df2_right = pd.DataFrame({'rKEY': ['K2', 'K3', 'K4', 'K5'],
    'C': ['C2', 'C3', 'C4', 'C5'],
    'D': ['D2', 'D3', 'D4', 'D5']})

>>> pd.merge(df2_left, df2_right, left_on='lKEY', right_on='rKEY')
 lKEY	A	  B	 rKEY	C	  D
0	K2	A2	B2	K2	C2	D2
1	K3	A3	B3	K3	C3	D3
```
* how
default는 `inner`로 교집합 결과를 반환한다.
``` python
# how 옵션
ineer : 교집합
outer : 합집합
left : 왼쪽 테이블 키 기준
right : 오른쪽 테이블 키 기준

>>> pd.merge(df_left, df_right, how='outer')
 KEY	A	  B	  C 	D
0	K0	A0	B0	NaN	NaN
1	K1	A1	B1	NaN	NaN
2	K2	A2	B2	C2	D2
3	K3	A3	B3	C3	D3
4	K4	NaN	NaN	C4	D4
5	K5	NaN	NaN	C5	D5
```
* suffixes
병합 시 동일한 column이 있을 경우 column명 처리에 관한 옵션
``` python
df_left = pd.DataFrame({'KEY': ['K0', 'K1', 'K2', 'K3'],
    'com': ['one','two','one','two'],
    'A': ['A0', 'A1', 'A2', 'A3'],
    'B': ['B0', 'B1', 'B2', 'B3']})

df_right = pd.DataFrame({'KEY': ['K2', 'K3', 'K4', 'K5'],
    'com': ['one','one','two','two'],
    'C': ['C2', 'C3', 'C4', 'C5'],
    'D': ['D2', 'D3', 'D4', 'D5']})

>>> pd.merge(df_left, df_right, on='KEY', suffixes=('_left','_right'))
 KEY com_left	A	  B	com_right	C	  D
0	K2	one	    A2	B2	one	    C2	D2
1	K3	two	    A3	B3	one	    C3	D3
```
* left_index, right_index
병합하려는 key가 색인인 경우,
``` python
df_left = pd.DataFrame({'left':['A1','B1','C1']}, 
                       index=['a','b','c'])

df_right = pd.DataFrame({'right':['A2','B2','C2']}, 
                        index=['a','b','c'])

>>> pd.merge(df_left, df_right, left_index=True, right_index=True)
  left	right
a	 A1	   A2
b	 B1	   B2
c	 C1	   C2
```
## join
색인으로 병합할때는 join 메소드를 사용하면 더욱 편리하다.
``` python
df_left = pd.DataFrame({'left':['A1','B1','C1']}, 
                       index=['a','b','c'])

df_right = pd.DataFrame({'right':['A2','B2','C2']}, 
                        index=['a','b','c'])

>>> df_left.join(df_right)
  left	right
a	 A1	   A2
b	 B1	   B2
c	 C1	   C2	
```
## append
``` python
df1 = pd.DataFrame(np.arange(9).reshape(3,3), index=['a','b','c'],
                  columns=['one','two','three'])
data = pd.Series(['a1','a2','a3'], index=['one','two','three'])

# df.append(series)
>>> df1.append(data, ignore_index=True)
	one	two	three
0	 0	 1	 2
1	 3	 4	 5
2	 6	 7	 8
3	 a1	 a2	 a3

# df.append(df)
df2 = pd.DataFrame(5+np.arange(9).reshape(3,3), index=['a','b','c'],
                  columns=['one','two','three'])

>>> df1.append(df2)
	one	two	three
a	 0	 1	 2
b	 3	 4	 5
c	 6	 7	 8
a	 5	 6	 7
b	 8	 9	 10
c	 11	 12	 13
```
## concat

concat은 데이터를 이어붙인다.
``` python
s1 = pd.Series([0,1], index=['a','b'])
s2 = pd.Series([2,3,4], index=['c','d','e'])
s3 = pd.Series([5,6], index=['f','g'])

>>> pd.concat([s1,s2,s3])
a    0
b    1
c    2
d    3
e    4
f    5
g    6

# axis
>>> s4 = pd.concat([s1,s2,s3], axis=1)
	 0	 1 	 2
a	0.0	NaN	NaN
b	1.0	NaN	NaN
c	NaN	2.0	NaN
d	NaN	3.0	NaN
e	NaN	4.0	NaN
f	NaN	NaN	5.0
g	NaN	NaN	6.0

# join
>>> pd.concat([s1,s4], axis=1, join='inner') # 교집합
	0	1
a	0	0
b	1	1
```
* keys
계층적 색인을 생성할 수 있다.
``` python
s1 = pd.Series([0,1], index=['a','b'])
s2 = pd.Series([2,3,4], index=['c','d','e'])
s3 = pd.Series([5,6], index=['f','g'])
s4 = pd.concat([s1,s3])

>>> pd.concat([s1,s2,s3], keys=['one','two','three'])
one    a    0
       b    1
two    c    2
       d    3
       e    4
three  f    5
       g    6
```
* DataFrame
``` python
df1 = pd.DataFrame(np.arange(6).reshape(3,2), index=['a','b','c'],
                  columns=['one','two'])
df2 = pd.DataFrame(5+np.arange(4).reshape(2,2), index=['a','c'],
                  columns=['three','four'])

>>> pd.concat([df1,df2], axis=1, keys=['level1','level2'])
>>> pd.concat({'level1':df1, 'level2':df2}, axis=1)
	level1	level2
  one	two	three	four
a	 0	 1	 5.0	 6.0
b	 2	 3	 NaN	 NaN
c	 4	 5	 7.0	 8.0
```
* ignore_index
index가 불필요한 경우, 무시하고 concat하기
``` python
df1 = pd.DataFrame(np.arange(4).reshape(2,2), columns=['one','two'])
df2 = pd.DataFrame(5+np.arange(4).reshape(2,2), columns=['one','three'])

>>> pd.concat([df1,df2], ignore_index=True)
  one	two	three
0	 0	1.0	 NaN
1	 2	3.0	 NaN
2	 5	NaN	 6.0
3	 7	NaN	 8.0
```
## combine_first
``` python
a = pd.Series([np.nan, 25, np.nan, 3.5, 4.5, np.nan],
             index=['f','e','d','c','b','a'])

b = pd.Series(np.arange(len(a), dtype=np.float64),
             index=['f','e','d','c','b','a'])

>>> np.where(pd.isnull(a),b,a)
>>> b[:-2].combine_first(a[2:]) # 위와 동일한 기능을 제공하며 자동으로 정렬해준다.
a    NaN
b    4.5
c    3.0
d    2.0
e    1.0
f    0.0
```