# 함수 정의 / 호출
``` python
# 함수 정의
def 함수명(매개변수):
  pass
	return True

# 함수 호출
함수명(매개변수)

# 함수 객체
함수명
<class 'function'>
```

# 인자
## 위치 인자
함수의 매개변수와 같은 순서로 인자 전달
## 키워드 인자
함수의 매개변수명에 맞춰 인자를 지정하여 전달
## default 매개변수
매겨변수에 기본값을 지정. 인자가 전달되지 않으면 해당 기본값을 사용.
## 위치 인자 모으기 : *
애스터리스크(*)는 매개변수에서 위치 인자 변수들을 튜플로 묶음.
``` python
def print_more(required1, requierd2, *args):
	print('Need this one:', required1)
	print('Need this one too:', required2)
	print('All the rest:', args)

>>>print_more('cap', 'gloves', 'scarf', 'monocle', 'mustache wax')
Need this one: cap
Need this one too: gloves
All the rest: ('scarf', 'monocle', 'mustache wax')
```
## 키워드 인자 모으기: **
`**`는 인자의 이름은 key이고, 값은 키에 대응하는 값이다.
``` python
def print_kwargs(**kwargs):
	print('Keyword arguments:', kwargs)

>>> print_kwargs(wine='merlot', entree='mutton', dessert='macaroon')
Keyword arguments: {'dessert':'macaroon', 'wine':'merlot', 'entree':'mutton'}
```

# docstring

함수 정의에 문서를 붙일수 있다.

``` python
def function():
  """
  docstring
  """
  
 help(function) : 함수 인자의 리스트와 docstring 출력
 function.__doc__ : 서식 없는 docstring을 그대로 출력
```

# 함수 객체

파이썬에서는 함수를 변수에 할당할 수 있고, 다른 함수에서 이를 인자로 쓸 수 있으며, 함수에서 이를 반환할 수 있다는 것이다.

## 내부함수와 클로져
클로져 : 내부함수가 외부함수의 context에 접근할 수 있는 것.
외부함수는 외부함수의 지역변수를 사용하는 내부함수가 소멸될 때까지 소멸되지 않는 특성을 의미

# 익명함수 : lambda
lambda 매개변수 : 식
``` python
>>> (lambda x : x + 10)(5)
15

>>> func = lambda x : x + 10
>>> func(5)
15
```

# map
map 함수는 리스트의 각 요소에 함수를 적용한다.
``` python
#형식: map(함수, 데이터)
num = ['1','2','3']
>>> map(int, num)
1 2 3

>>> x, y = map(int, input("문자열 두개 입력:").split())
x+y
```




