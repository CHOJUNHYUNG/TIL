# Generic

## setdefault() , defaultdict()
setdefault : 누락된 key / value값을 할당. 존재하는 key값은 원래 값을 반환하고 딕셔너리에는 변화 없음.
``` python
periodic_table = {'Hydrogen':1,'Helium':2}
carborn = periodic_table.setdefault('Carborn',12)
>>> periodic_table
{'Hydrogen':1,'Helium':2, 'Carborn':12}
```
defaultdict() : 모든 value를 기본값으로 지정
``` python
from collections import defaultdict
periodic_table = defaultdict(int)
periodic_table['Hydrogen'] = 1
periodic_table['Lead']
>>> periodic_table
defaultdict(<class 'int'>,{'Lead':0, 'Hydrogen':1})
```
```python
bestiary = defaultdict(lambda: 'Huh?')
>>> bestiary['E']
'Huh?'
```

## Counter()
```python
from collections import Counter
breakfast = ['spam','spam','egg','spam']
breakfast_counter = Counter(breakfast)
>>> breakfast_counter
Counter({'spam':3, 'egg':1})

>>> breakfast_counter.most_common() # 내림차순 반환 , 해당 갯수의 요소 반환
[('spam',3), ('egg', 1)]
>>> breakfast_counter.most_common(1)
[('spam',3)]
```
또한 +, -, &, | 연산자를 통해 두 Counter들 간의 연산도 가능하다.

## OrderedDict()
딕셔너리는 원래 키의 순서가 없는 클래스이다. 
OrderedDict는 키의 추가 순서를 기억하고, 순서대로 값을 반환한다.
``` python
from collections import OrderedDict
que = OrderedDict([
	('Moe', 1),
	('Larry', 2)
])
>>> for q in que:
			print(q)
Moe
Larry
```

## Stack + Queue = Deque
데크는 스택과 큐의 기능을 모두 가지고 있는 자료구조이다. 시퀀스의 양 끝에서 항목을 추가, 삭제할 수 있다.
popleft()와 pop()으로 각각 왼쪽과 오른쪽에서 항목을 반환할 수 있다.
``` python
from collections import deque
word = 'racecar'
dq = deque(word)
```

## itertools
``` python
import itertools
# chain함수는 순환 가능한 인자를 하나씩 반환
for item in itertools.chain([1,2],['a','b']):
	print(item, end=" ")
1 2 a b

# cycle함수는 무한 이터레이터.
for item in itertools.chain([1,2]):
	print(item, end=" ")
1 2 1 2 1 2 .....

# accumulate함수는 누적합을 계산.
for item in itertools.accumulate([1,2,3,4]):
	print(item, end=" ")
1 3 6 10
## 두 번째 인자로 함수를 주면 해당 함수로 누적 계산
def multiply(a,b):
	return a*b
	
for item in itertools.accumulate([1,2,3,4], multiply):
	print(item, end=" ")
1 2 6 24
```

## pprint()
출력을 좀 더 깔끔하게 해준다.
``` python
from pprint import pprint
```