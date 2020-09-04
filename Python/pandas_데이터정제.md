# 결측값

## dropna
``` python
# DataFrame.dropna는 NA값이 하나라도 있다면 행 전체를 drop한다.
df.dropna()

# how='all'은 모두 NA인 경우만 drop한다. 또한 옵션에 axis를 줄 수 있다.
df.dropna(how='all')
df.dropna(axis=1)
```
## fillna
``` python
# 결측값을 채우고자 하는 값을 인자로 준다.
df.fillna(0)

# dict를 활용하여 column별로 다른 값을 채울 수 있다.
df.fillna({'col1':0, 'col2':0.5})

# ffill/bfill 옵션을 줄 수 있다.
df.fillna(method='ffill')
```

# 데이터 변형
## duplicated / drop_duplicates
``` python
# duplicated는 각 row의 중복 여부를 반환한다.
>>> df.duplicated()
row1 False
row2 False
...

# drop_duplicates는 duplicated가 False인 DataFrame을 반환. 기준열을 지정할 수 있다.
df.drop_duplicates(['col1'])

# keep 옵션으로 남길 위치의 중복값을 선택할 수 있다.
df.drop_duplicates(['col1','col2'], keep='last') # 마지막 값을 남김.
```
## 함수와 map
map은 Series 객체에 적용할 수 있다. map함수를 활용하여 데이터 값을 기반으로 데이터를 변형할 수 있다.
``` python
lowercase = df['col1'].str.lower()
lowercase.map(data)

# 위와 동일
df['col1'].map(lambda x: data[x.lower()])

# dict로 mapping
map_dict = {'a':0,'b':1,'c':2}
df2['alpha'] = df2['alpha'].map(map_dict)
```
## replace
``` python
# data.replace(목표값, 대체값)
data.replace(-999, np.nan)

# 여러 값을 지정할 수 있다.
data.replace([-999,1000], [np.nan,0])
data.replace({-999: np.nan, -1000: 0})
```
## rename : 축 색인 바꾸기
``` python
transformer = lambda x: x[:4].upper()
data.index = data.index.map(transformer)

# 변경된 새로운 객체를 생성하기
data.rename(index=str.title, columns=str.upper)
# 색인의 일부만 변경 거능
data.rename(index={'row1':'R1'}, columns={'col1':'C1'})
```
## cut : Categoriacal
``` python
# pd.cut(목표값, 구간)
# cut 함수를 통해 categorical 객체를 만들 수 있다.
bins = [18,25,35,60,100] # 10~25, 25~35, 35~60. 60~100 구간 설정
cats = pd.cut(data, bins)

# categories 확인하기
>>> cats.code
array([0, 0, 1, 2, 0])

>>> cats.categories
IntervalIndex([(18,25],(25,35],...])

>>> pd.value_counts(cats)
(18,25]  3
(25,35]  10
 ...
 
# 구간 범위 변경
cats = pd.cut(data, bins, right=False)
[[18, 26),[26,36),...]

# 그룹명 지정
names = ['name1', 'name2',...]
pd.cut(ages, bins, labels=names)

# cut 함수에 구간 범위를 명시하지 않으면 최대, 최소값을 기준으로 균등하게 나눈다.
# 4개 구간으로 균등 분할. precision은 소수점을 2자리로 제한한다.
pd.cut(data, 4, precision=2)
```
## qcut
cut은 데이터의 분산에 따라 나누지만 qcut은 표준 변위치를 사용하기 때문에 임의의 범위로 데이터를 나누기에 적절하다.
``` python
cats = pd.qcut(data, 4, labels=['Q1','Q2','Q3','Q4']) # 4분위로 분류

# 변위치를 직접 지정할 수도 있다. 0 ~ 1
cats = pd.qcut(data,[0, 0.1, 0.5, 0.9, 1])
```

## 샘플링

* np.random.permutatioin
``` python
sample = np.random.permutation(len(data))

>>> sample
[3, 1, 4, 2, 0]

# 데이터 취하기
df.take(sample)
df.iloc[sample,:]
```
* sample
``` python
# n개의 표본 샘플링
df.sample(n=3)

# 중복허용하여 샘플링 : replace=True
df.sample(n=3, replace=True)
```
## get_dummies : 더미 변수
``` python
dummy_df = pd.get_dummies(df['key'])

# 접두어 붙이기
dummy_df = pd.get_dummies(df['key'], prefix='key')

## cut과 get_dummies 활용하기
pd.get_dummies(pd.cut(data,bins))
```
## 벡터화 문자열

``` python
data.strs.[메서드]
```
[string handling](https://pandas.pydata.org/pandas-docs/version/0.25.3/reference/series.html)

