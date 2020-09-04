``` python
import os
```
# 파일
## 존재여부 확인 :  exists
``` python
os.path.exists('test.txt')
```
## 타입 확인 : isfile, isdir, isabs
``` python
os.path.isfile('test.txt')

os.path.isdir(name) # 디렉토리 확인

os.path.isabs(name) # 절대경로인지 확인
```
## 복사, 이동 : copy , move
``` python
import shutil

shutil.copy('test.txt','test2.txt') # 복사
shutil.move('test.txt','test2.txt') # 이동. 원본 파일은 삭제
```
## 이름 바꾸기 : rename
``` python
os.rename('test.txt','test2.txt')
```
## 연결하기 : link , symlink
``` python
# hard link
os.link('test.txt','hard_test.txt')

# symbolic link (soft link)
os.symlink('test.txt','soft_test.txt')

# link 끊기
os.unlink('hard_test.txt')
```
## 퍼미션 바꾸기 : chmod
읽기, 쓰기 등 권한 설정
``` python
import stat
os.chmod('test.txt', stat.S_IRUSR)
```
## 오너십 바꾸기 : chown
사용자 아이디와 그룹 아이디를 지정하여 파일의 소유자와 그룹에 대한 오너십을 지정. 유닉스/리눅스 기반에서 사용.
``` python
uid = 5
gid = 22
os.chown('test.txt', uid, gid)
```
## 절대 경로 얻기 : abspath
``` python
os.path.abspath('test.txt')
# 현재 디렉토리.../test.txt
```
## 심벌릭링크 경로 얻기
symlink로 만들어진 파일의 실제 경로를 얻을 수 있다.
``` python
os.path.realpath('soft_test.txt')
```
## 삭제하기 : remove
``` python
os.remove('test.txt')
```

# 디렉토리
## 생성하기 , 삭제하기
``` python
# 생성하기
os.mkdir('dir name')
# 삭제하기
os.rmdir('dir name')
```
## 목록 불러오기 : listdir
``` python
os.listdir('dir name')
```
## 현재 디렉토리 바꾸기 : chdir
``` python
os.chdir('변경할 path')
```
## 일치하는 파일 나열 : glob
`*` : 모든 것 일치
? : 한 문자 일치
[ab] : a, b 문자에 일치
[!ab] : a, b문자 제외하고 일치

``` python
import glob

glob.glob('*.txt')
glob.glob('file?.*')
glob.glob('**', recursive=True) # 하위 디렉토리까지 모두 탐색
```


# 프로그램 / 프로세스
## 프로세스 생성 : subprocess
쉘의 실행 결과를 스크립트로 가져올 수 있고 파일로 저장할 수 있도록 해준다.
Subprocess 모듈은 새로운 프로세스를 실행하도록 도와주고, 프로세스의 입출력 및 에러 결과에 대한 리턴코드를 받도록 해준다.
``` python
import subprocess

# getoutput : 실행 결과 얻기
ret = subprocess.getoutput('test.py')
>>> ret
python test

# check_output : 실행 결과 얻기. 바이트 타입을 반환.
ret = subprocess.check_output(['test.py'], encoding='utf-8')

# getstatusoutput : 상태 코드와 결과를 튜플로 반환
ret = subprocess.getstatusoutput('test.py')
>>> ret
(0, python test)

# call() : 상태 코드만을 저장
ret = subprocess.call('test.py')
>>> ret
0
```
## 프로세스 생성 : multiprocessing
파이썬 함수를 별도의 프로세스로 실행하거나 한 프로그램에서 독립적인 여러 프로세스를 실행할 수 있다.
``` python
import multiprocessing

p = multiprocessing.Process()
```
## 프로세스 종료 : terminate
``` python
import multiprocessing

p = multiprocessing.Process()
...
p.terminate()
```


# 날짜 / 시간
## datetime 모듈
datatime 모듈의 객체
* date : 년, 월, 일
* time : 시, 분, 초, 마이크로초
* datetime : 날짜, 시간
* timedelta : 날짜, 시간 간격
## time 모듈
time 모듈은 1970년 1월 1일 자정 이후 시간의 초를 사용.
``` python
import time

# 현재 시간의 epoch값
now = time.time()

# epoch값을 문자열로 변환
time.time(now)

# 시간 요소 얻기 : localtime / gmtime
time.localtime(now) # 시스템의 표준시간대
time.gmtime(now) # UTC 시간
```
## 날짜 포맷팅 : strftime / strptime
* strftime : 날짜/시간을 문자열로 변환
* strptime : 문자열을 날짜/시간으로 변환

자세한 출력 지정자는 [strftime 형식 지정자](https://strftime.org) 참조

