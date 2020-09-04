# 정규표현식 메타문자와 패턴
## 메타문자 
: 원래 그 문자가 가진 뜻이 아닌 특별한 용도로 사용되는 문자.
* [   ] : 문자 클래스. [ ] 사이의 문자들과 매치
* Dot (.) : `\n`을 제외한 모든 문자를 의미
* ^ : 해당 문자열이 맨 앞에 오는지 판단
* $ : 해당 문자열이 맨 뒤에 오는지 판단
* `*` : 해당 문자열이 0개 이상 있는지 판단
* `+` : 해당 문자열이 1개 이상 있는지 판단
* ? : 해당 문자열이 0 또는 1개만 잇는지 판단
* {반복횟수} : 해당 문자열의 반복 횟수 판단
* | :' 또는'의 의미
* `\` : 특수문자를 표현하고자 할때 앞에 붙임
## 패턴 , 특수문자
* [A-Z] : 알파벳 대문자 모두
* [a-z] : 알파벳 소문자 모두
* [가-핳] : 한글 모두
* [0-9] : 숫자 모두 =. \d 
* [^0-9] : 숫자를 제외한 모두  = \D
* \w : 숫자 + 문자 모두 = [0-9a-zA-Z] 
* \W : 숫자 + 문자 제외한 모두
* \s : whitespace (공백문자)
* \S : 비공백문자
* \b : 단어 경계 (\w와 \W사이의 경계)
* \B : 비단어 경계

``` python
# . 와 [.]의 차이
re.compile("a.b") = (a + 아무글자 + b)를 매치
re.compile("a[.]b") = '.' 그대로의 의미. (a.b)를 매치

# {a:b} 횟수 지정
re.compile("a{3}") = a가 3번 반복
re.compile("a{3:5}") = a가 3~5번 반복

# ^[0-9]와 [^0-9]의 차이
re.compile("^[0-9]") = 문자열의 맨 앞이 숫자로 시작
re.compile("[^0-9]") = 숫자를 제외한 모두

# Non-Greedy : ? (*, +, { })뒤에 ?가 온 경우
re.compile("[a-z]*?") = 해당 문자열이 매치되면 이후의 탐색을 종료
re.compile("[a-z]+?")
re.compile("[a-z]{3:6}?")

# 전방 탐색 : (?=) 긍정형 / (?!) 부정형
re.compile("prev(?=next)") = next인 경우 prev를 반환
re.compile("prev(?!next)") = next가 아닌 경우 prev를 반환

# 후방 탐색 : (?<=) 긍정형 / (?<!) 부정형
re.compile("(?<=prev) next") = prev인 경우 next 반환
re.compile("(?<!prev) next") = prev가 아닌 경우 next 반환
```
## Raw-string
raw-string은 \를 해석하지 않고 남겨둔다.
raw-string을 사용하지 않고 \section을 패턴으로 지정하고 싶다면
``` python
re.compile('\\\\section') # 파이썬 리터럴 규칙에 의해 \\ => \로 전달되기 때문에
```
다음과 같을 것이다. 하지만 raw-string을 사용하면
``` python
re.compile(r'\\section')
```
과 같이 간단하게 표현할 수 있다.

# 정규표현식 표준 모듈과 컴파일

``` python
import re

# 패턴과 소스의 일치여부 확인
result = re.match('You', 'Young Frankenstein')
# 패턴 compile
pattern = re.compile('You')
result = pattern.match('Young Frankenstein')
```
## 컴파일 옵션
* DOTALL, S : Dot(.)을 \n을 포함하여 매치함.
``` python
p = re.compile('a.b', re.DOTALL)
p = re.compile('a.b', re.S)
```
* IGNORECASE, I : 대소문자 구별 없이 매치함.
``` python
p = re.compile('[a-z]', re.IGNORECASE)
p = re.compile('[a-z]', re.I)
```
* MULTILINE, M : 문자열이 여러줄일 경우, 각각을 하나의 줄로 인식하여 매치함. ^, $ 옵션과 관련이 있음
``` python
p = re.compile('^Python', re.MULTILINE)
p = re.compile('^Python', re.M)
```
* VERBOSE , X : 패턴에 주석을 입력할 수 있게 해줌. 이 옵션을 사용하면 문자열에 사용된 공백을 제거하고 컴파일함. ([ ]안의 공백은 제외). 주석은 `#`을 이용해 달 수 있음.
``` python
charref = re.compile(r"""
 &[#]                # Start of a numeric entity reference
 (
     0[0-7]+         # Octal form
   | [0-9]+          # Decimal form
   | x[0-9a-fA-F]+   # Hexadecimal form
 )
 ;                   # Trailing semicolon
""", re.VERBOSE)
```

# 메소드
* match( ) : 문자열의 처음부터 일치하는 패턴 찾기
* search( ) : 문자열 전체에서 첫 번째로 일치하는 패턴 찾기
* findall( ) : 일치하는 모든 패턴 찾기
* finditer( ) : 일치하는 모든 패터을 반복 가능한 객체로 반환
* split( ) : 패턴으로 문자열 구분하기. 리스트를 반환.
* sub( ) : 일치하는 패턴 대체하기
``` python
import re
re.match("[0-9]*","123abc456") # <re.Match object; span=(0, 2), match='123'>
re.search("[0-9]*$","123abc456") # <re.Match object; span=(6, 9), match='456'>
```
``` python
pnt = re.compile("[a-z]+")
    
# search 
m = pnt.search("9python")
print(m)   # <re.Match object; span=(1, 7), match='python'>
m = pnt.search("9python 7 java")
print(m)   # <re.Match object; span=(1, 7), match='python'>

# findall
res = p.findall("Life is too short")
print(res)   # ['ife', 'is', 'too', 'short']

# finditer
res = p.finditer("Life is too short")
for r in res:
    print(r.start()) # 매치 시작위치  # 1 ....
    print(r.end())   # 매치 끝 위치   # 4 ... 
    print(r.group()) # 매치 문자열    # ife ...
    print(r.span())  # (시작, 끝)    # (1,4) ...
```
``` python
pnt = re.compile('n')
# split
m = pnt.split("Young Frankenstein")
>>> print(m)
['You', 'g Fra', 'ke', 'stei']

# sub
m = re.sub('n','?', 'Young Frankenstein')
>>> m
'You?g Fra?ke?stei?'

# sub에 함수 적용하기
def toHex(mat):
    val = int(mat.group())
    return hex(val) 

pnt = re.compile("\d+")
>>> pnt.sub(toHex,"call 114, 99 for user code")
'call 0x72, 0x63 for user code'
```
# Group
``` python
source = """ I wish I may, I wish I might
Have a dish of fish tonight."""

# 패턴 결과 매칭
m = re.search(r'(. dish\b).*(\bfish)', source)
>>> m.group()
'a dish of fish'
>>> m.groups()
('a dish', 'fish')
>>> m.group(0)
'a dish'
>>> m.group(1)
'fish'

# 그룹 이름 지정
m = re.search(r'(?P<DISH>. dish\b).*(?P<FISH>\bfish)', source)
>>> m.gorup('DISH')
'a dish'
>>> m.group('FISH')
'fish'
```

