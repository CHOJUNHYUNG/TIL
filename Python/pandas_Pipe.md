# 메서드 연결 기법
데이터 셋을 여러 차례 변형해야 하는 경우 분석에 전혀 필요 없는 임시 변수를 계속 생성하는 상황이 발생한다. 
## assign
assign은 값 대입이 완료된 새로운 DataFrame을 반환한다.
``` python
df2 = df.copy()
df2['k'] = v

# 위와 동일한 기능의 실용적인 코드
df2 = df.assign(k=v)
```
* 예제
``` python
result = (df2.assign(col1_demeaned=df2.col1 - df2.col2.mean()).groupby('key').col1_demeaned.std())
```
## 임시 객체 참조
``` python
df = load_data()
df2 = df[df['col2'] < 0]

# 위와 동일한 기능의 실용적인 코드
df = (load_data()[lambda x: x['col2'] < 0])
```
* 예제
``` python
result = ((load_data()[lambda x: x['col2'] < 0]).assign(col1_demeaned=df2.col1 - df2.col2.mean()).groupby('key').col1_demeaned.std())
```

# pipe 메서드
``` python
a = f(df, arg1=v1)
b = g(a, v2, arg3=v3)
c = h(b, arg4=v4)

df.pipe(f, arg1=v1).pipe(g, v2, arg3=v3).pipe(h, arg4=v4)
```
여기서 `f(df)`와 `df.pipe(f)`는 동일하다.
* 예제
``` python
g = df.groupby(['key1','key2'])
df['col1'] = df['col1'] - g.transform('mean')
```
``` python
def group_demean(df, by, cols):
  result = df.copy()
  g = df.groupby(by)
  for c in cols:
    result[c] = df[c] - g[c].transform('mean')
 	return result

result = df[df.col1 < 0].pipe(group_demean, ['key1','key2'], ['col1'])
```

