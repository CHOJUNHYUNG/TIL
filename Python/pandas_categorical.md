# Categorical 형
특정값의 반복으로 인한 메모리 낭비를 줄여 성능을 개선시킬 수 있다.
``` python
fruits = ['apple', 'orange', 'apple', 'apple'] * 2
N = len(fruits)
df = pd.DataFrame({'id':np.arange(N),'fruit':fruits})

>>> fruit_cat = df['fruit'].astype('category')
0     apple
1    orange
...
7     apple
Name: fruit, dtype: category
Categories (2, object): [apple, orange]

c = fruit_cat.values
>>> type(c)
pandas.core.arrays.categorical.Categorical

>>> c.categories
Index(['apple', 'orange'], dtype='object')

>>> c.codes
array([0, 1, 0, 0, 0, 1, 0, 0], dtype=int8)
```
``` python
my_categories = pd.Categorical(['foo','bar','baz','foo','bar'])

# from_codes. 순서를 보장하지 않음 => ordered 옵션
categories = ['foo','bar','baz']
codes = [0,1,2,0,0,1]
my_categories2 = pd.Categorical.from_codes(codes, categories, ordered=True)
```
## categorical 메서드
``` python
s = pd.Series(['a','b','c','d']*2)
cat_s = s.astype('category')

>>> cat_s.cat.codes
>>> cat_s.cat.categories

# set_categories : 실제 카테고리 범주를 알고 있는 경우
actucal_cat = ['a','b','c','d','e']
>>> cat_s_2 = cat_s.cat.set_categories(actual_cat)

# remvoe_unused_categories : 관측되지 않는 카테고리 제거
cat_s3 = cat_s.cat.remove_ununsed_categories()

# 기타 메서드
add_categories : 기존 카테고리에 새로운 카테고리 추가
as_ordered : 순서를 가진 카테고리
as_unordered : 순서가 없는 카테고리
remove_categories : 카테고리 제거. 제거된 카테고리는 null값으로 대체.
remvoe_unused_categories : 관측되지 않는 카테고리 제거
rename_categories : 카테고리 이름 지정
```
## get_dummies
One-Hot encoding.
``` python
>>> pd.get_dummies(cat_s)
	a	b	c	d
0	1	0	0	0
1	0	1	0	0
2	0	0	1	0
3	0	0	0	1
4	1	0	0	0
5	0	1	0	0
6	0	0	1	0
7	0	0	0	1
```

