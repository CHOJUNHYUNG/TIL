# 그룹 변환과 Groupby 객체 풀어내기
transform : apply와 유사하게 동작하면서 사용할 수 있는 함수의 종류에 대해 좀 더 맣은 제한을 포함할 수 있다.
``` python
df = pd.DataFrame({'key':['a','b','c']*4, 'value': np.arange(12.)})
g = df.groupby('key').value
>>> g.mean()
key
a    4.5
b    5.5
c    6.5

# 그룹의 'key'에 따른 각 평균값
>>> g.transform(lambda x:x.mean())
>>> g.transform('mean')
0     4.5
1     5.5
2     6.5
3     4.5
4     5.5
5     6.5
6     4.5
7     5.5
8     6.5
9     4.5
10    5.5
11    6.5
```
``` python
def normalize(x):
  return (x - x.mean()) / x.std()

>>> g.transform(normalize)
>>> g.apply(normalize)
0    -1.161895
1    -1.161895
2    -1.161895
3    -0.387298
4    -0.387298
5    -0.387298
6     0.387298
7     0.387298
8     0.387298
9     1.161895
10    1.161895
11    1.161895
```
## groupby 연산을 벡터 연산으로 풀어내기
``` python
>>> normalized = (df['value'] - g.transform('mean')) / g.transform('std')
0    -1.161895
1    -1.161895
2    -1.161895
3    -0.387298
4    -0.387298
5    -0.387298
6     0.387298
7     0.387298
8     0.387298
9     1.161895
10    1.161895
11    1.161895
```
