# 리스트와 튜플
리스트와 튜플은 파이썬 시퀀스이다.
이들은 문자열 시퀀스와 달리 각 항목의 타입이 모두 다를 수 있다.
튜플은 불변하고, 리스트는 가변하다.

# 리스트

## 기본 조작

``` python
# 리스트 생성
empty_list = list()
weekdays = ['Mon','Tue','Wen','Thu','Fri','Sat','Sun']
list('cat')
>>> ['c','a','t']
birth = '1/1/1990'
birth.split('/')
>>> ['1','1','1990']

# 항목 얻기 / 바꾸기
marxes = ['Groucho','Chico','Harpo']
marxes[0]
>>> 'Groucho'
marxes[2] = 'Wanda'
>>> ['Groucho','Chico','Wanda']

# 이중 리스트
d_list = [[1,2,3],['a','b','c']]
len(d_list)
>>> 2
d_list[0]
>>> [1,2,3]
len(d_list[0])
>>> 3
```
## 메소드

### 리스트 항목 추가 : append()

``` python
other = ['Gummo','Karl']

marxes.append('Zeppo')
>>> ['Groucho','Chico','Harpo','Zeppo']

marxes.append(other)
>>> ['Groucho','Chico','Harpo',['Gummo','Karl'] ]
```

### 리스트 병합 : extend() / +
``` python
marxes.extend(other)
>>> ['Groucho','Chico','Harpo','Gummo','Karl']
marxes += ohter
>>> ['Groucho','Chico','Harpo','Gummo','Karl']
```
### 리스트 항목 추가 : insert()
``` python
marxes.insert(2,'Gummo') # (index , 항목)
>>> ['Groucho','Chico','Gummo','Harpo']
```
### 항목 삭제 : del / remove()
``` python
del marxes[1]
>>> ['Groucho','Harpo']

del marxes # 변수 제거

marxes.remove('Chico')
>>> ['Groucho','Harpo']
```
### 항목 얻기 : pop()
``` python
marxes.pop()
>>> 'Harpo'

marxes.pop(1) # index
>>> 'Chico'
```
### 인덱스 얻기 : index()
``` python
marxes.index('Chico')
>>> 1
```
### 존재여부 확인 : in
``` python
'Groucho' in marxes
>>> True
```
### 값 세기 : count()
``` python
marxes.count('Chico')
>>> 1
marxes.count('Bob')
>>> 0
```
### 문자열 변환 : join()
``` python
', '.join(marxes)
>>> 'Groucho, Chico, Harpo'
```
### 정렬하기 : sort()
sort : 리스트 자체를 내부적으로 정렬
sorted : 정렬된 리스트 복사본 반환
``` python
num_list = [2,4,3,1]

num_list.sort()
num_list
>>> [1,2,3,4]

sorted(num_list)
>>> [1,2,3,4]
```
### 항목 개수 : len()
``` python
len(marxes)
>>> 3
```
### 할당 / 복사
`a = [1,2,3]`은 할당이므로 `b = a`에서 b의 요소를 변경하면 a에 할당된 리스트에서도 값이 변경된다.
새로운 리스트를 생성하기 위해서는 리스트를 복사해야한다.

``` python
# 1. copy()
b = a.copy()
# 2. list() 함수
c = list(a)
# 3. 슬라이싱
d = a[:]
```

# 튜플
튜플은 리스트처럼 값을 변경할 수는 없고 사용할 수 있는 메소드 또한 적다. 튜플은 다음과 같은 상황에 유용하다* 적은 공간을 사용
* 항목이 손상될 염려가 없다.
* 딕셔너리의 키로 사용 가능
* 네임드 튜플
* 함수의 인자로 전달

## 튜플 생성하기
``` python
empty_tuple = ()
empty_tuple = tuple()

one_marx = 'Groucho',
one_marx
>>> ('Groucho',)

marx_tuple = 'Groucho', 'Chico', 'Harpo'
marx_tuple
>>> ('Groucho', 'Chico', 'Harpo')

marx_tuple = ('Groucho', 'Chico', 'Harpo')
marx_tuple
>>> ('Groucho', 'Chico', 'Harpo')
```
## 튜플 언패킹
``` python
G , C , H = ('Groucho', 'Chico', 'Harpo')
```


# 딕셔너리
key - value 쌍으로 구성된 자료구조
다른 언어에서는 연관 배열, 해시, 해시뱁이라고 부른다.
key는 유일한 값들을 의미한다.

## 딕셔너리 생성
``` python
empty_dict = {}
empty_dict = dict()

dict = {'key':value,'key':value,}
```
## 리스트/튜플의 딕셔너리 변환
``` python
L2D = [['a','b'],['c','d'],['e','f']] # [('a','b'),('c','d'),('e','f')]
T2D = (['a','b'],['c','d'],['e','f']) 
dict(L2D) 
>>> {'c':'d', 'a':'b', 'e':'f'}

S2D = ['ab','cd','ef'] # ('ab','cd','ef')
dict(S2D)
>>> {'c':'d', 'a':'b', 'e':'f'}
```
## 항목 추가/변경
``` python
python = {'Chapman':'Graham','Cleese':'John','Idle':'Eric'}

# dict[key] = value
# 추가 
python['Gilliam'] = 'Gerry'
# 변경
python['Gilliam'] = 'Terry'
```
## 딕셔너리 결합 : update()
``` python
other = {'Marx':'Groucho', 'Howard':'Moe'}

python.update(other)
>>> {'Chapman':'Graham','Cleese':'John','Idle':'Eric', 'Marx':'Groucho', 'Howard':'Moe'}
# 값이 중복되면 두 번째 딕셔너리 값 저장
```
## 항목 삭제하기 : del / clear()
``` python
del python['key']

python.clear() # 딕셔너리의 모든 항목 삭제
```
## Key/Value 얻기 : get, keys, values, items
``` python
python['chapman']
>>> 'Graham' # key가 존재하지 않으면 error
python.get('chapman')
>>> 'Graham'
python.get('Marx', 'Not a Python')
>>> 'Not a Python'

# 모든 key값 얻기
python.keys()

# 모든 value값 얻기
python.values()

# key-value 쌍 얻기
python.items()
>>> (key,value)
```
## key 존재여부 확인 : in
``` python
'Chapman' in python
>>> True
```


# 셋
key만 남은 딕셔너리와 같다. 즉 유일한 값들을 갖는 자료구조.
## set 생성 밎 변환
``` python
empty_set = set()

set('python')
>>> {'p','y','t','h','o','n'}

set(리스트/셋/딕셔너리)
>>> {셋}
```
## 존재여부 확인 : in
## 교집합 / 합집합 / 차집합
``` python 
a = {1,2}
b = {2,3}
# 교집합 : & , intersection()
a & b / a.intersection
>> {2}
# 합집합 : | , union()
a | b / a.union(b)
>>> {1,2,3}
# 차집합 : - , difference()
a - b / a.difference(b)
>>> {1}

# 대칭 차집합 : ^ , symmetric_difference(); 한쪽에는 있지만 양쪽 모두에 없는 멤버
a ^ b / a.symmetric_difference(b)
>>> {1,3}

# 부분집합 : <= , issubset()
a <= b / a.issubset(b)
>>> False
# 진부분집합 : < ; 두 번째 셋에는 첫 번째 셋의 모든 멤버를 포함하고 그 이상의 멤버가 있다. 프로퍼 서브셋
a < b
>>> False

# 슈퍼셋 : >= , issuperset() ; 서브셋의 반대
a >= b
>>> False
# 프로퍼 슈퍼셋
a > b
>>> False
```