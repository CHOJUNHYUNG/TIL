# 제네레이터, 데커레이터
* 제너레이터 :
파이썬 시퀀스를 생성하는 객체. 전체 시퀀스를 메모리에 저장하는 것이 아닌 잠재적으로 순회가능한 시퀀스를 생성.
range()와 같이 예약된 제네레이터가 있고 사용자가 직접 정의할 수도 있다. 직접 정의시 `yield`로 값을 반환.

``` python
def generator(first=0, last=10, step=1):
	number = first
	while number < last:
		yield number
		number += step
    
ranger = generator(1,5)
>>> type(ranger)
<generator object generator ...>

>>> for x in ranger:
  			print(x, end=" ")
1 2 3 4
```
## itertools
* 무한 반복자
``` python
import itertools
# count(start, step)
>>> itertools.count(10, 2)
10 12 14 16 ...

# cycle(obj)
>>> itertools.cycle('ABC')
ABCABCABC...

# repeat(elem, n)
>>> itertools.repeat(10)
10 10 10 ...
>>> itertools.repeat(10, 3)
10 10 10 
```
* 종료 반복자
``` python
import itertools
# accumulate(p, func)
>>> itertools.accumulate([1,2,3,4,5])
1 3 6 10 15

# chain(p, q)
>>> itertools.chain('ABC', 'DEF')
ABCDEF

# chain.from_iterable()
>>> itertools.chain.from_iterable(repeat([[1,1],[2,2],[3,3]]))
1 1 2 2 3 3

# compress(data, selector)
>>> itertools.compress('ABC', [1,0,1])
AC

# takewhile - 조건이 참일때까지 반복
>>> itertools.takewhile(lambda x: x<5, [1,4,6,3,1])
1 4

# dropwhile - 조건이 참이 아닐때부터 반복
>>> itertools.dropwhile(lambda x: x<5, [1,4,6,3,1])
6 3 1

# filterfalse
>>> itertools.filterfalse(lambda x: x%2, range(10))
0 2 4 6 8

# islice(seq, [start], end, [step])
>>> itertools.islice('ABCDEFG',2, None, 2)
C E

# starmap(func, seq)
>>> itertools.starmap(pow, [(2,5), (3,2), (10,3)])
32 9 1000

# zip_longest()
>>> itertools.zip_longest('ABCD', 'XY', fillvalue='-')
AX BY C- D-
```
``` python
# groupby(iterable, key)
d = [('Europe','Paris'),
     ('Asia','Seoul'),
     ('Europe','London'),
     ('Asia','Beijing'),]
category = {}

for k, g in itertools.groupby(sorted(d), lambda x:x[0]):
  listg =[x[1] for x in list(g)] 
  category[k] = listg
  
>>> category
{'Asia':['Seoul', 'Beijing'], 'Europe':['Paris', 'London']}
```
* 조합 반복자
``` python
# product
>>> itertools.product('ABC',repeat=2)
('A','A'), ('A','B'), ('A','C'), ('B','A'), ('B','B'), ...

# permutations
>>> itertools.permutations('ABC',r=2)
('A','B'), ('A','C'), ('B','A'), ('B','C'), ('C','A'), ('C','B')

# combinations
>>> itertools.combinations('ABC',r=2)
('A', 'B'), ('A', 'C'), ('B', 'C')

# combinations_with_replacement
>>> itertools.combinations_with_replacement('ABC',r=2)
('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'B'), ('B', 'C'), ('C', 'C')
```

* 데커레이터 :
하나의 함수를 취해 또 다른 함수를 반환하는 함수.
`*args`, `**kwargs`, 내부함수, 함수 인자를 활용하여 정의.
``` python
# 데커레이터 정의
def documents_it(func):
	def new_function(*args,**kwargs):
		print('Running function:', func.__name__)
		print('Positional arguments:', args)
		print('Keyword arguments:', kwargs)
		result = func(*args, **kwargs)
		print('Result:', result)
		return result
	return new_function
# 함수 정의
def add(a, b):
	return a + b
```
``` python
# 데커레이터 수동으로 할당
>>> cooler_add = documents_it(add)
>>> cooler_add(3, 5)
Running function: add
Positional arguments: (3, 5)
Keyword arguments: {}
Result: 8
8

# @로 할당
@documents_it
def add(a, b):
	return a + b

>>> add(3, 5)
Running function: add
Positional arguments: (3, 5)
Keyword arguments: {}
Result: 8
8
```
또한 함수는 여러개의 데커레이터를 가질 수 있다. 이때 함수로부터 가까운 데커레이터부터 실행된다.
``` python
# 새로운 데커레이터 정의
def square_it(func):
  def new_function(*args, **kwargs):
    result = func(*args, **kwargs)
    return result * result
  return new_function

# square_it -> documents_it
@documents_it
@square_it
def add(a, b):
  return a + b
>>> add(3, 5)
Running function: add
Positional arguments: (3, 5)
Keyword arguments: {}
Result: 64
64

# documents_it -> square_it
@square_it
@documents_it
def add(a, b):
  return a + b
>>> add(3, 5)
Running function: add
Positional arguments: (3, 5)
Keyword arguments: {}
Result: 8
64
```

# 네임스페이스, 스코프
네임스페이스 ~ 지역 변수와 전역 변수
``` python
def func():
	animal # 지역 변수
	global animal # 전역 변수

__main__:
animal # 전역 변수
```
# _와 __
__ : 예약된 변수에 사용

_ : 다양한 상황에서 사용됨.
1. 인터프리터의 마지막 값을 저장할때
``` python
>>> 10
10
>>> _
10
>>> _ * 3
30
>>> _ * 20
600
```
2. 값을 무시할때
``` python
>>> x , *_ , y = (1, 2, 3, 4, 5) # 여러개 값 무시
x = 1 , y = 5

for _ in range(1,10)
```
3. 특별한 의미의 네이밍
	1. `_single_leading_underscore`: 주로 한 모듈 내부에서만 사용하는 private 클래스/함수/변수/메서드를 선언할 때 사용하는 컨벤션이다. 이 컨벤션으로 선언하게 되면 `from module import *`시 _로 시작하는 것들은 모두 임포트에서 무시된다. 그러나, 파이썬은 진정한 의미의 private을 지원하고 있지는 않기 때문에 private을 완전히 강제할 수는 없다. 즉, 위와 같은 임포트문에서는 무시되지만 직접 가져다 쓰거나 호출을 할 경우엔 사용이 가능하다. 그래서 “weak internal use indicator"라고 부르기도 한다.
	2. `single_trailing_underscore_`: 파이썬 키워드와의 충돌을 피하기 위해 사용하는 컨벤션이다.
	
	  ``` python 
	  list_ = List.objects.get(1) # list 키워드와의 충돌 방지
	  ```
	3. `__double_leading_underscores`: 더블 언더스코어는 클래스 속성명을 맹글링하여 클래스간 속성명의 충돌을 방지하기 위한 용도로 사용된다. (맹글링이란, 컴파일러나 인터프리터가 변수/함수명을 그대로 사용하지 않고 일정한 규칙에 의해 변형시키는 것을 말한다.) 파이썬의 맹글링 규칙은 더블 언더스코어로 지정된 속성명 앞에 _ClassName을 결합하는 방식이다.
	
	```python 
	class A:
	    def _single_method(self):
	        pass
	    def __double_method(self): # 맹글링을 위한 메서드
	        pass
	
	class B(A):
	    def __double_method(self): # 맹글링을 위한 메서드
	        pass
	
	print(dir(A())) # ['_A_double_method', ..., '_single_method']
	print(dir(B())) # ['_A_double_method', '_B_double_method', ..., '_single_method']
	# 서로 같은 이름의 메서드를 가지지만 오버라이드가 되지 않는다.
	```
	4. `__double_leading_and_trailing_underscores__`: 스페셜 변수나 메서드(매직 메서드라고도 부른다.)에 사용되는 컨벤션이며, __init__, __len__과 같은 메서드들이 있다. 이런 형태의 메서드들은 어떤 특정한 문법적 기능을 제공하거나 특정한 일을 수행한다. 가령, __file__은 현재 파이썬 파일의 위치를 나타내는 스페셜 변수이며, __eq__은 a == b라는 식이 수행될 때 실행되는 스페셜 메서드이다.
	


3. 국제화(i18n) / 지역화(I10n) 함수로 사용
4. 숫자의 자릿수 구분
``` python
dec_number = 10_000_000
bin_number = 0b_1111_0000

>>> print(dec_number)
10000000
>>> print(bin_numbr)
240
```

# 예외처리
``` python
try:
	실행문
  # 실행문에서 에러가 발생하면 except문 실행
except:
	예외 발생시 실행문
	# 예외가 발생하지 않으면 except문은 pass
```

## 예외 핸들러
``` python
short_list = [1, 2, 3]
while True:
  value = input('Position [q to quit]? ')
  if value = 'q':
    break
	try:
    position = int(value)
    print(short_list[position])
	except IndexError as err:
    print('Bad index:', position)
	except Exception as other:
    print('Something else broke:', other)
```

## 예외 만들기

예외는 class이고 , Exception 클래스의 자식 클래스이다.

``` python
class UppercaseException(Exception):
  pass

words = ['eeenie', 'MO']
for word in words:
  if word.isupper():
    raise UppercaseException(word)
```

# 자기관찰
변수의 이름 앞이나 뒤에 `?`를 붙이면 그 객체에 대한 정보를 출력한다.
`??`를 사용하면 가능한 경우에 한해 소스코드를 보여준다.

# %run / %load
IPython 환경에서 `%run`은 세션안에서 파이썬 파일(py)을 불러와 실행시켜준다. 스크립트에 정의된 모든 변수는 실행된 수 접근가능하다. 파이썬 파일에서 IPython에 이미 선언된 변수에 접근할때는 `%run -i`를 사용한다.
주피터 노트북에서는 `%load`를 사용할 수 있다.
