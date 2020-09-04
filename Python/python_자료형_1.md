# 데이터 타입

* 부울 : True / False
* 정수
* 실수
* 문자열

# 객체

파이썬에서는 모든 것(부울, 정수, 실수, 문자열, 데이터 구조, 함수, 프로그램 )들이 객체로 구현되어있어 언어 일관성등 유요한 기능을 제공한다.
객체는 `데이터`가 담긴 공간(상자)과 같다. 또한 객체는 데이터와 함께 `타입`을 갖는다.
데이터의 값이 가변(변수)이건 불변(상수)이건 상관없이 파이썬은 객체의 타입을 바꿀 수 없는 강타입이다.
객체는 `데이터(변수,속성:attribute)`와 `코드(함수, 메서드)`를 포함한다.


# 변수

변수는 메모리의 값을 참조하기 위한 이름이다. 프로그래밍 언어에서는 변수를 선언하고 값을 할당한다. 
`할당`한다는 것은 값을 복사하는 것이 아닌 객체에 이름을 붙여주는 것이다.

``` python
a = 7 # 변수명 = 객체

print(a)
>>> 7 

type(a)
>>> <class 'int'>
```

# 숫자
정수 / 부동소수점수 지원
## 연산자

``` python
# 더하기
5 + 8
>>> 13
# 빼기
5 - 8
>>> -3
# 곱하기
5 * 8
>>> 40
# 나누기
## 부동소수점 나누기
5 / 8
>>> 0.625
## 정수나누기
5 // 8
>>> 0
# 나머지
5 % 8
>>> 5
# 지수
5 ** 8
>>> 390625
# divmod
divmod(7,3)
>>> (2,1) # 몫, 나머지
```

## 진수

2진수 : 0b / 0B
8진수 : 0o / 0O
16진수 : 0x / 0X

## 형 변환
다른 데이터 타입을 정수형으로 변환하기. > int( ) 함수
다른 데이터 타입을 부동소수점형으로 변환하기. > float( ) 함수

```python
int(True)
>>> 1

int(98.6)
>>> 98

int('98.6')
>>> Error
```

## int 크기
python 3에서는 int의 크기가 유연해졌다. 심지어 구골(10**100)의 값도 담을 수 있다.



# 문자열
문자열 = 파이썬 시퀀스
## 형 변환
str( ) 함수 사용.
## 이스케이프 문자 (\)
\n : 줄바꿈 , \t : tab , \\" : "문자로 인식 ... 

## 문자열 연산

* 결합 : + 
```python 
  'Hi' + 'Hello'
  >>> 'HiHello'
```

* 복제하기 : *
``` python
'Hi' * 5
>>> HiHiHiHiHi
```

## 인덱싱 / 슬라이싱

``` python
string = 'abcdefghijklmnopqrstuvwxyz'
# 인덱싱
string[3]
>>> 'c'
string[-3]
>>> 'x'
# 슬라이싱 [start:end:step]
string[20:]
>>> 'uvwxyz'
string[12:15]
>>> 'mno'
string[-3:]
>>> 'xyz'
string[::7]
>>> 'ahov'
string[::-1]
>>> 'zyxwvutsrqponmlkjihgfedcba'
```
## 문자열 길이 : len()
``` python
len(string)
>>> 26
```

## 문자열 나누기 : split() , sep
주어진 인자를 기준으로 문자열 구분. 인자가 없으면 공백으로 구분
```python
string = 'get gloves,get mask,give cat vitamins'

string.split(',')
>>> ['get gloves','get mask','give cat vitamins']
string.split()
>>> ['get','gloves,get','mask,give','cat','vitamins']

print(1,2,3,4,sep=',,,')
>>>1,,,2,,,3,,,4
```

## 문자열 결합하기 : join()
``` python
string_list = ['get gloves','get mask','give cat vitamins']

','.join(string)
>>> 'get gloves,get mask,give cat vitamins'
```

## 문자열 잘라내기 : strip()

``` python
string = '   Hello...   '

string.strip() # 양쪽 공백 제거
>>> 'Hello...'
string.lstrip() # 왼쪽 공백 제거
>>> 'Hello...   '
string.rstrip() # 오른쪽 공백 제거
>>> '   Hello...'

string = 'Hello...'
string.strip('.') # . 제거
>>> 'Hello'
```
## 문자열 대체하기 : replace()
``` python
string = 'a Duck goes into a bar'

string.replace('Duck','marmoset')
>>> 'a marmoset goes into a bar'
string.replace('a','a famous',100) # 100회로 제한
>>> 'a famous Duck goes into a famous bar'
```
## 문자열 입력받기 : input()
``` python
inp = input('입력:')

print(inp)
>>> 5
type(inp)
>>> str
```
## 다양한 문자열 함수

### startswitch / endswitch
``` python
text = """All that doth flow we cannot liquid name
Or else would fire and water be the same;
But 'tis the wet that makes it die, no doubt.'
"""

text.startswitch('All') # 'All'로 시작하는가?
>>> True
text.endswitch('That\'s all, folks!') # 'That\'s all, folks!'로 끝나는가?
>>> False
```

### index / find / rfind / count / isalnum

``` python
word = 'the'

text.index('l') # 앞에서부터 찾아 인덱스 반환 # 업으면 error
>>> 1
text.rindex('l') # 뒤에서부터 찾아 인덱스 반환
>>> 52	
text.find(word) # 처음 the가 나오는 오프셋  # 없으면 -1
>>> 73
text.rfind(word) # 마지막으로 the가 나오는 오프셋
>>> 92
text.count(word) # the가 몇번 나오는가?
>>> 2
text.isalnum() # 문자, 숫자로만 이루어져있는가?
>>> False
```

### 대소문자 변환
``` python
string = 'a Duck goes into a bar'

string.capitalize() # 첫 문자만 대문자로
>>> 'A Duck goes into a bar'
string.title() # 모든 단어의 첫 문자를 대문자로
>>> 'A Duck Goes Into A Bar'
string.upper() # 모든 문자를 대문자로
>>> 'A DUCK GOES INTO A BAR'
string.lower() # 모든 문자를 소문자로
>>> 'a duck goes into a bar'
string.sawpcase() # 대문자-소문자 swap
>>> 'A dUCK GOES INTO A BAR'
string.center(30) # 지정 공간의 중앙에 배치
>>> '  a Duck goes into a bar  '
string.ljuct(30) # 지정 공간의 왼쪽에 배치 cf) rjust : 오른쪽 배치
>>> 'a Duck goes into a bar    '
```

### ljust, rjust
``` python
string.ljust() # 왼쪽 정렬
string.rjust() # 오른쪽 정렬
```