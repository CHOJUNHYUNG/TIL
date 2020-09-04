# 모듈
모듈 : 함수나 변수, 클래스 등을 모아 놓은 py 파일
다른 파이썬 프로그램에서 모듈을 사용하기 편하게 하기 위해서 사용
``` python
<mod1.py>
def add(a,b):
    return a+b
def sub(a,b):
    return a-b

if __name__ == "__main__": # 해당 파일에서만 동작하도록 하는 조건
    print(add(2,3))
    print(sub(4,2))
    
"""
 __name__ == "__main__":을 사용하면 조건이 참이어서 if문에 있는 문장수행
그런데 외부 파일에서 이 모듈을 불러와 실행할 때는 if __name__ != "__main__":이므로 if문이 실행이 안됨
"""
```
``` python
<mod2.py>
import mod1 # import 묘듈명
print(mod1.add(2,3)) # 모듈이름. 함수이름()
-------------------------------------------------
from mod1 import add, sub # from 모듈이름 import 모듈함수
# from mod1 import *
print(add(1,2))
print(sub(1,2))
```
# 패키지
패키지 : 모듈들을 모아 놓은 디렉토리
패키지 디렉토리에는 `__init__.py`가 필요함. 이 파일이 있는 디렉토리를 패키지로 간주함.
같은 프로젝트 내의 파일, external libraries 내의 패키지도 import 가능

``` python
source
|------ __init.py
|------ daily.py
|------ weekly.py
```
```python
from source import *
daily.dforecast()
weekly.wforecast()
-------------------------------------------------
from source.daily import dforecast
from source.weekly import wforecast
daily.dforecast()
weekly.wforecast()
```