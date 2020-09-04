# Groupby
그룹 연산 : 분리(split) - 적용(apply) - 결합(combine)
* 분리(split) : 하나 이상의 키를 기준으로 분리
* 적용(apply) : 분리 후 각 그룹에 함수 적용
* 결합(combine) : 적용 결과를 하나의 객체로 결합
``` python
df = pd.DataFrame({'key1' : ['a','a','b','b','a'],
                   'key2' : ['one','two','one','two','one'],
                   'data1': np.random.randn(5),
                   'data2': np.random.randn(5)})

>>> grouped = df['data1'].groupby(df['key1']) # groupby 객체 생성
<pandas.core.groupby.generic.SeriesGroupBy object at 0x7f9fea85c5c0>
```
``` python
# 집계 메서드 사용
>>> grouped.mean()
key1
a    0.205361
b   -0.476375

# 두 개의 색인으로 묶은 계층적 색인을 갖는 객체 생성
>>> df['data1'].groupby([df['key1'],df['key2']]).mean()
key1  key2
a     one     0.165869
      two     0.284343
b     one    -0.691938
      two    -0.260812
```
``` python
# dataframe에서 집계 연산은 숫자 데이터를 처리한다. 숫자 데이터가 아닌 경우 집계에서 제외시킨다.(성가신 컬럼)
>>> df.groupby('key1').mean()
	     data1	    data2
key1		
a	   0.205361	  -0.810490
b	   -0.476375	0.978859

>>> df.groupby(['key1','key2']).mean()
		         data1	   data2
key1	key2		
a	    one	  0.165869	-1.404741
      two	  0.284343	 0.378013
b	    one	 -0.691938	 0.052252
      two	 -0.260812	 1.905465
```
* size 메서드
``` python
>>> df.groupby(['key1','key2']).size()
key1  key2
a     one     2
      two     1
b     one     1
      two     1
```
## 그룹 간 순회
* groupby 객체는 이터레이션을 지원.
``` python
>>> for name, group in df.groupby('key1'):
    	print(name)
    	print(group)
a
    key1 key2     data1     data2
0    a  one  1.951922 -0.956766
1    a  two -2.438222  0.787492
4    a  one  0.268934  0.504911
b
   key1 key2     data1     data2
2    b  one -0.241784  0.023424
3    b  two -1.294484  0.837230

>>> for dtype, group in df.groupby(df.dtypes, axis=1):
    	print(dtype)
    	print(group)

float64
      data1     data2
0  1.951922 -0.956766
1 -2.438222  0.787492
...
object
  key1 key2
0    a  one
1    a  two
...
```
* 일부만 선택
``` python
>>> df.groupby('key1')[['data1']]
<pandas.core.groupby.generic.DataFrameGroupBy object at 0x7fa1514c3c18>

>>> df.groupby(['key1','key2'])[['data2']].mean()
             data2 
key1	key2	
a	    one	 -0.225928
      two	 0.787492
b	    one	 0.023424
      two	 0.837230
```
## 그룹핑
* dictionary, Series
``` python
people = pd.DataFrame(np.random.randn(5,5),
                     columns=['a','b','c','d','e'],
                     index=['Joe','Steve','Wes','Jim','Travis'])
people.iloc[2:3,[1,2]] = np.nan

mapping = {'a':'red', 'b':'red', 'c':'blue', 'd':'blue', 'e':'red', 'f':'orange'}
map_series = pd.Series(mapping)
```
``` python
>>> people.groupby(mapping, axis=1).count()
     blue	red
Joe	   2	3
Steve	 2	3
Wes	   1	2
Jim	   2	3
Travis 2	3

>>> people.groupby(map_series, axis=1).count()
     blue	red
Joe	   2	3
Steve	 2	3
Wes	   1	2
Jim	   2	3
Travis 2	3
```
* 함수
``` python
>>> people.groupby(len).sum()
	   a	        b       	c        	d	       e
3	0.147760	0.467585	0.931455	-0.557066	0.082109
5	1.050313	2.625978	1.100666	1.383860	1.168627
6	0.079307	0.021443	-0.266437	0.720587	-0.197434

key_list = ['one','one','one','two','two']
>>> people.groupby([len, key_list]).sum()
	         a	      b	        c	        d	       e
3	one	-0.565672	0.280948	0.861766	0.615608	2.252013
  two	0.713432	0.186637	0.069689	-1.172675	-2.169904
5	one	1.050313	2.625978	1.100666	1.383860	1.168627
6	two	0.079307	0.021443	-0.266437	0.720587	-0.197434
```
* 색인 단계
``` python
columns = pd.MultiIndex.from_arrays([['US','US','US','JP','JP'],
                                    [1,3,5,1,3]],
                                    names=['cty','tenor'])

hier_df = pd.DataFrame(np.random.randn(4,5), columns=columns)
>>> hier_df.groupby(level='cty', axis=1).count()
cty	JP	US
0	   2	3
1	   2	3
2	   2	3
3	   2	3
```
# 데이터 집계
groupby 메서드에는 count, sum, mean, median, std, prod 등의 최적화된 메서드들이 있다.
이 외에도 정의된 메서드를 활용할 수 있다.
``` python
df = pd.DataFrame({'key1' : ['a','a','b','b','a'],
                   'key2' : ['one','two','one','two','one'],
                   'data1': np.random.randn(5),
                   'data2': np.random.randn(5)})

# 정의된 메서드 사용
>>> df.groupby('key1')['data1'].quantile(0.9)
key1
a    0.647738
b    1.276631

# 사용자 정의 함수 사용 : aggregate / agg()
def peak_to_peak(arr):
  return arr.max() - arr.min()

>>> df.groupby('key1').agg(peak_to_peak)
        data1	    data2
key1		
a	    2.210296	1.996706
b	    0.437242	1.606415
```
## column에 집계 함수 적용
* column마다 적용
``` python
func = ['mean',peak_to_peak]
>>> df.groupby(['key1','key2']).agg(func)
          data1	              data2
          count	peak_to_peak	count	peak_to_peak
key1 key2				
a	   one	 2	     1.624092	    2	    1.761042
     two	 1	     0.000000	    1	    0.000000
b	   one	 1	     0.000000	    1	    0.000000
     two	 1	     0.000000	    1	    0.000000
  
# column이름 지정 : (이름, 함수)
>>> df.groupby(['key1','key2']).agg([('수량','mean'),('범위',peak_to_peak)])
          data1	            data2
          수량	범위	        수량	 범위
key1 key2				
a	   one	 2	 1.624092	    2	    1.761042
     two	 1	 0.000000	    1	    0.000000
b	   one	 1	 0.000000	    1	    0.000000
     two	 1	 0.000000	    1	    0.000000
```
* column별로 적용
``` python
>>> df.groupby(['key1','key2']).agg({'data1':['sum','mean'], 'data2':'count'})
	           data1	          data2
             sum	    mean	  count
key1	key2			
a	    one	1.686620	0.843310	  2
      two	1.412240	1.412240	  1
b	    one	1.983691	1.983691	  1
      two	-0.908724	-0.908724	  1
```
* 색인 없이 반환하기 : as_index=False
``` python
>>> df.groupby(['key1','key2'], as_index=False).count()
  key1 key2	data1	data2
0	 a	 one	 2	    2
1	 a	 two	 1	    1
2	 b	 one	 1	    1
3	 b	 two	 1	    1 
```

# Apply
일반적으로 groupby 메서드의 목적은 apply이다.
apply는 groupby에 의해 나뉜 각 조각에 함수를 일괄 적용한 후 합친다.
``` python
df = pd.DataFrame({'key1' : ['a','a','b','b','a'],
                   'key2' : ['one','two','one','two','one'],
                   'data1': np.random.randn(5),
                   'data2': np.random.randn(5)})

def top(df, n=2, columns='data1'):
    return df.sort_values(by=columns)[-n:]
```
``` python
>>> df.groupby('key1').apply(top)
	     key1	key2	data1	     data2
key1					
 a	1	  a	  two	 1.412240	  -0.207716
    4	  a	  one	 1.655356	  -1.429285
 b	3	  b	  two	 -0.908724	-1.157856
    2	  b	  one	 1.983691	  -0.437439

# 함수의 인자 전달
>>> df.groupby(['key1','key2']).apply(top, n=1, columns='data2')
			      key1	key2	 data1	   data2
key1	key2					
a	    one	0	 a	  one	 0.031264	  0.331757
      two	1	 a	  two	 1.412240	 -0.207716
b	    one	2	 b	  one	 1.983691	 -0.437439
      two	3	 b	  two	-0.908724	 -1.157856
```
* 그룹 색인 생략 : group_keys=False
``` python
>>> df.groupby('key1', group_keys=False).apply(top)
  key1	key2	  data1	    data2
1	 a	  two	  1.412240	-0.207716
4	 a	  one	  1.655356	-1.429285
3  b	  two	 -0.908724	-1.157856
2	 b	  one	  1.983691	-0.437439
```
