# 계층적 색인
* Series
```python
data = pd.Series(np.random.randn(9),
                index=[['a','a','a','b','b','c','c','d','d'],
                       [1,2,3,1,3,1,2,2,3]])
>>> data
a 1  -0.204
  2   0.478
  3  -0.519
b 1  -0.555
  3   1.967
c 1   1.395
  2   0.929
d 2   0.281
  3   0.768

>>> data.index
MultiIndex(levels=[['a','b','c','d'],[1,2,3]],
           labels=[[0,0,0,1,1,2,2,3,3],[0,1,2,0,2,0,1,1,2]])
```
데이터에 부분적 색인으로 접근가능하며, 하위 계층의 객체를 선택할수 도 있다.
``` python
# 부분적 색인으로 접근하기
data['b']
data['b':'c']
data[['b','d']]

# 하위 계층으로 접근
data.loc[:,2]
a  0.478
c  0.929
d  0.281
```
* DataFrame
``` python
df = pd.DataFrame(np.arange(12).reshape((4,3)),
                 index=[['a','a','b','b'],[1,2,1,2]],
                 columns=[['Ohio','Ohio','Colorado'],
                          ['Green','Red','Green']])

>>> df
    Ohio       Colorado
    Green Red  Green
a 1   0    1     2
  2   3    4     5
b 1   6    7     8
  2   9    10    11
  
# DataFrame 계층적 색인의 name 지정 가능
df.index.names = ['key1','key2']
df.columns.names = ['state','color']

# 계층적 색인의 접근
>>> df['Ohio']
color     Green Red
key1 key2
a    1      0    1
     2      3    4
b    1      6    7
     2      9    10
```

* set_index / reset_index
set_index : 지정한 색인으로 새로운 DataFrame 생성.
``` python
df = pd.DataFrame({'a':range(5), 'b':range(5,0,-1),
                  'c':['one','one','two','two','two'],
                  'd':[0,1,0,1,2]})
>>> df
  a b  c  d
0 0 5 one 0
1 1 4 one 1
2 2 3 two 0
3 3 2 two 1
4 4 1 two 2

>>> df2 = df.set_index(['c','d'])
       a  b
 c  d 
one 0  0  5
one 1  1  4
two 0  2  3
two 1  3  2
two 2  4  1

>>> df2.reset_index()
  a b  c  d
0 0 5 one 0
1 1 4 one 1
2 2 3 two 0
3 3 2 two 1
4 4 1 two 2
```
## swaplevel
계층의 순서를 바꾸거나 지정된 계층에 따라 데이터를 정렬해야할 경우 활용할 수 있다.
``` python
>>> df.swaplevel('key2', 'key1')
state    Ohio       Colorado
color    Green Red  Green
key2 key1
1     a   0    1     2
2     a   3    4     5
1     b   6    7     8
2     b   9    10    11

>>> df.swaplevel('key2', 'key1').sort_index(level=0)
state    Ohio       Colorado
color    Green Red  Green
key2 key1
1     a   0    1     2
1     b   6    7     8
2     a   3    4     5
2     b   9    10    11
```
## 계층별 요약 통계
Series와 DataFrame의 통계 메소드의 level 옵션을 통해 계층별로 통계값을 구할 수 있다.
``` python
>>> df.sum(level='key2')
state    Ohio       Colorado
color    Green Red  Green
key2
1         6    8     10
2         12   14    16

>>> df.sum(level='color', axis=1)
color      Green  Red
key1 key2
a    1       2     1
     2       8     4
b    1       14    7
     2       20    10
```


