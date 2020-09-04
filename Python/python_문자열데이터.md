# 파이썬 텍스트 문자열

## 1. 유니코드
바이트(byte) : 컴퓨터의 기본 저장 단위. 1바이트 = 8비트 = 2^8(256)개의 데이터를 저장.
영어는 ASCII 코드로 모두 표현이 가능. 비유럽권 언어를 표현하기 위해 유니코드를 정의.
[유니코드 코드 차트 페이지](https://unicode.org/charts/) , [유니코드 평면](https://ko.wikipedia.org/wiki/유니코드_평면)

* UTF-8 인코딩과 디코딩
파이썬에서 외부 데이터를 교활할 때는 인코딩/디코딩 과정이 필요하다.
UTF-8는 동적 인코딩 형식으로 유니코드 한 문자당 1~4바이트를 사용한다.
문자열 => 바이트 : 인코딩
바이트 => 문자열 : 디코딩
### 인코딩
encode(인코딩 이름, 예외 처리)
``` python
snowman = '\u2603'
ds = snowman.encode('utf-8')
>>> len(ds)
3
>>> ds
b'\xe2\x98\x83
```
예외 처리:
ignore : 알 수 없는 문자 무시
replace : 알 수 없는 문자 '?'로 대체
backslashreplace : 파이썬 유니코드 문자의 문자열을 생성
xmlcharrefreplace : 유니코드 이스케이프 시퀀스를 출력할 수 있는 문자열로 생성
``` python
>>> snowman.encode('ascii', 'ignore')
b''
>>> snowman.encode('ascii', 'replace')
b'?'
>>> snowman.encode('ascii', 'backslashreplace')
b'\\u2603'
>>> snowman.encode('ascii', 'xmlcharrefreplace')
b'&#9731'
```
### 디코딩


## 2. 포맷
* % 스타일
| %s   | 문자열 | %f   | 부동소수점수         |
| ---- | ------ | ---- | -------------------- |
| %d   | 10진수 | %e   | 지수                 |
| %x   | 16진수 | %g   | 부동소수점수 or 지수 |
| %o   | 8진수  | %%   | 리터럴 %             |
``` python
n = 42
f = 7.03
s = 'python'

# 기본 포맷팅
>>> '%d %f %s' %(n, f, s)
42 7.030000 python

# 자리수 포맷팅, 오른쪽 정렬
>>> '%10d %10f %10s' %(n, f, s)
      42       7.030000       python
  
# 자리수 포맷팅, 왼쪽 정렬
>>> '%-10d %-10f %-10s' %(n, f, s)
42       7.030000       python

# 자리수 및 출력, 소수점 이하 자릿수 포맷팅
>>> '%10.4d %10.4f %10.4s' %(n, f, s)
'      0042    7.0300    python'

# 인자로 필드 길이 지정
>>> '%*.*d %*.*f %*.*s' %(10, 4, n, 10, 4, f, 10, 4, s)
'      0042    7.0300    python'
```
* { }, format 스타일
``` python
# 기본 포맷팅
>>> '{} {} {}'.format(n, f, s)
42 7.03 python

# 순서 지정
>>> '{2} {0} {1}'.format(f, s, n)
42 7.03 python

d = {'n':42, 'f':7.03, 's':'python'}
>>> '{0[n]} {0[f]} {0[s]}'.format(d)
42 7.03 python

>>> '{n} {f} {s}'.format(n=42, f=7.03, s='python')
42 7.030000 python

# 형식 포맷팅
>>> '{0:d} {1:f} {2:s}'.format(n, f, s)
42 7.030000 python

# 자릿수 포맷팅
>>> '{0:10d} {1:10f} {2:10s}'.format(n, f, s)
     42     7.030000       python
  
# 정렬
## > : 오른쪽 정렬 , < : 왼쪽 정렬 , ^ : 가운데 정렬
>>> '{0:>10d} {1:>10.4f} {2:>10.4s}'.format(n, f, s)
     42     7.030000       python
  
# 채워 넣기
>>> '{:=^10s}'.format(python)
==python==
```
* f' ' 스타일
f-string의 { }에는 변수, 계산식, 메소드, 클래스 객체 등의 모든 객체를 입력할 수 있다.
``` python
n = 42
f = 7.03
s = 'python'

>>> f'number={n}, float={f}, string={s}'
'number=42, float=7.03, string=python'
```

## [3. 정규표현식](./python_정규표현식.md)







