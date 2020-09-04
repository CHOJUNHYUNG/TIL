# 재형성과 피벗
## stack / unstack
``` python
data = pd.DataFrame(np.arange(6).reshape(2,3),
                   index=pd.Index(['Ohio','Colorado'], name='state'),
                   columns=pd.Index(['one','two','three'],name='number'))

>>> stacked = data.stack()
state     number
Ohio      one       0
          two       1
          three     2
Colorado  one       3
          two       4
          three     5
```
``` python
>>> stacked.unstack()
number	 one	two	three
state			
Ohio	    0	   1	  2
Colorado	3	   4	  5

# unstack 옵션 : 단계 지정. 기본적으로는 가장 안쪽의 레벨부터 unstack
>>> stacked.unstack(0)
>>> stacked.unstack('state')
```
## pivot
``` python
# pivot(row, column, value)
data = pd.DataFrame({'cust_id': ['c1', 'c1', 'c1', 'c2', 'c2', 'c2', 'c3', 'c3', 'c3'],
                  'prod_cd': ['p1', 'p2', 'p3', 'p1', 'p2', 'p3', 'p1', 'p2', 'p3'],
                  'pch_amt': [30, 10, 0, 40, 15, 30, 0, 0, 10]})

>>> data.pivot(index='cust_id',columns='prod_cd',values='pch_amt')
prod_cd	 p1	 p2	 p3
cust_id			
c1	     30	 10	 0
c2	     40	 15	 30
c3	     0	 0	 10

>>> data['pch_amt2'] = np.arange(len(data))
>>> data.pivot(index='cust_id',columns='prod_cd')
        pch_amt	    pch_amt2
prod_cd	p1	p2	p3	p1	p2	p3
cust_id						
c1	    30	10	0	  0	  1	  2
c2	    40	15	30	3	  4	  5
c3	    0	  0	  10	6	  7	  8
```
## pivot_table
``` python
# pivot_table(values, index, columns, aggfunc, fill_value, dropna, margins)

data = pd.DataFrame({'cust_id': ['c1', 'c1', 'c1', 'c2', 'c2', 'c2', 'c3', 'c3', 'c3'],
                  'prod_cd': ['p1', 'p2', 'p3', 'p1', 'p2', 'p3', 'p1', 'p2', 'p3'],
                   'grade' : ['A', 'A', 'A', 'A', 'A', 'A', 'B', 'B', 'B'],
                  'pch_amt': [30, 10, 0, 40, 15, 30, 0, 0, 10]})

>>> data.pivot_table(['pch_amt'], index=['cust_id'], columns='prod_cd')
	      pch_amt
prod_cd	p1	p2	p3
cust_id			
c1	    30	10	0
c2	    40	15	30
c3	    0	  0	  10

# margins로 부분합 포함
>>> data.pivot_table(['pch_amt'], index=['cust_id'], columns='prod_cd', margins=True)
	      pch_amt
prod_cd	    p1	       p2	        p3	      All
cust_id				
c1	    30.000000	 10.000000	0.000000	 13.333333
c2	    40.000000	 15.000000	30.000000	 28.333333
c3	     0.000000	  0.000000	10.000000	  3.333333
All	    23.333333	 8.333333	  13.333333	 15.000000

# 집계 함수 설정
>>> data.pivot_table(['pch_amt'], index=['cust_id'], columns='prod_cd', aggfunc=sum)
	      pch_amt
prod_cd	p1	p2	p3
cust_id			
c1	    30	10	0
c2	    40	15	30
c3	     0	 0	10
```
## cross table
그룹의 빈도 계산을 위한 pivot table의 특수 경의
``` python
data = pd.DataFrame({'cust_id': ['c1', 'c1', 'c1', 'c2', 'c2', 'c2', 'c3', 'c3', 'c3'],
                  'prod_cd': ['p1', 'p2', 'p3', 'p1', 'p2', 'p3', 'p1', 'p2', 'p3'],
                   'grade' : ['A', 'A', 'A', 'A', 'A', 'A', 'B', 'B', 'B'],
                  'pch_amt': [30, 10, 0, 40, 15, 30, 0, 0, 10]})

>>> pd.crosstab(data.cust_id, data.grade)
grade	  A	B
cust_id		
c1	    3	0
c2	    3	0
c3	    0	3
```
## melt
``` python
df = pd.DataFrame({'key':['foo', 'bar', 'baz'],
                  'A':[1,2,3], 'B':[4,5,6], 'C':[7,8,9]})

>>> pd.melt(df,['key'])
	key	variable	value
0	foo	   A	       1
1	bar	   A	       2
2	baz	   A	       3
3	foo	   B	       4
4	bar	   B	       5
5	baz	   B	       6
6	foo	   C	       7
7	bar	   C 	       8 
8	baz	   C	       9
```