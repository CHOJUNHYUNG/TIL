# 파일 입출력
* 파일 열기
  `f = open('filename', 'mode')`
  mode : 
  * r - 파일 읽기
  * r+ - 일고 쓰기
  * w - 파일 쓰기 (파일이 존재하지 않으면 파일 생성, 존재하면 덮어쓰기)
  * w+ - 읽고 쓰기 (파일이 존재하지 않으면 파일 생성, 존재하면 덮어쓰기) 
  * x - 파일 쓰기 (파일이 존재하지 않을경우만)
  * a - 파일 추가하기 (쓰기 전용. 파일이 존재하면 파일의 끝에서부터 씀)
* a+ - 파일 추가하기 (읽기 가능)
  
  두번째 글자는 파일 타입을 명시함.
  
  * t (default) - 텍스트 타입
  * b - 이진 타입
  
* 파일 닫기
파일을 열고 다 사용하면 다시 닫아주어야한다.
`f.close()`

* 파일 열고 닫기
``` python
with open('filename','mode') as f:
	f.write()
```

* 텍스트 파일 쓰기
``` python
text = "Example Text"

f = open('test.txt','wt')
f.write(text)
f.close()

# print를 이용한 파일 쓰기
f = open('test.txt','wt', sep=' ', end=' ')
print(text, file=f)
f.close()
```
write는 스페이스나 줄 바꿈을 추가하지 않는다. 따라서 print가 write와 같이 동장하려면 sep와 end를 위와 같이 추가해주어야 한다.
* 텍스트 파일 읽기
**read**
전체 파일을 한 번에 읽어옴
``` python
text = ''
f = open('test.txt','rt')
text = f.read()
f.close()

# 한 번에 읽을 길이 지정
f = open('test.txt','rt')
chunk = 100
while True:
  frag = f.read(chunk)
  if not frag:
    break
   text += frag
f.close()
```
**readline**
파일을 라인 단위로 읽어옴.
``` python
f = open('test.txt','rt')
while True:
  line = f.readline()
  if not line:
    break
   text += line
f.close()
```
**readlines**
한 번에 모든 라인을 읽어 들이고, 한 라인으로 된 문자열 리스트를 반환한다.
``` python
f = open('test.txt','rt')
lines = f.readlines()
f.close()
>>> lines
['Example Text\n']
```

* 파일 위치 찾기 : tell , seek
  **tell** : tell 함수는 파일의 시작으로부터 현재 오프셋을 바이트 단위로 반환한다.
  **seek** : seek 함수는 해당 바이트 오프셋으로 위치를 이동시킨다.
``` python
f = open('test.txt','r')
>>> f.tell()
0
>>> f.seek(7)
>>> f.tell()
7
```
* `seek(offset, origin)`
origin = 0 : 시작 위치에서 offset 만큼 이동
origin = 1 : 현재 위치에서 offset 만큼 이동
origin = 2 : 마지막 위치에서 offset 만큼 이전으로 이동

# pickle
직렬화 : 객체를 파일로 저장하는 것을 직렬화라고 한다.
``` python
import pickle

# 직렬화
pickled = pickle.dumps(obj)	
# 역직렬화
pickle.load(pickled)

# 파일로 저장하기
f = open('pickle.pkl','wb')
pickle.dump(obj, f)
f.close()
# 파일에서 불러오기
f = open('pickle.pkl','rb')
obj = pickle.load(f)
f.close()
```

# 데이터 파일
## 구조화된 텍스트 파일
* CSV
``` python
import csv

# 파일 쓰기
text = [['test1'], ['test2'],]
f = open('test.csv','w+')
csvwriter = csv.writer(f)
csvwriter.writerows(text)

# 파일 읽기
with open('test.csv','r') as f:
	fcsv = csv.reader(f)
	text = [row for row in fcsv]

# 딕셔너리로 읽기
with open('test.csv','r') as f:
	fcsv = csv.DictReader(f, fieldnames=['test'])
	text = [row for row in fcsv]
```
* XML
``` python
import xml.etree.ElementTree as et

tree = et.ElementTree(file='파일명')
root = tree.getroot()
for child in root:
	print(child.tag, child.arrtib)
```
* HTML
* JSON
``` python
import json
```
* YAML
``` python
import yaml
```

## 구조화된 이진 파일
* 스프레드시트
* HDF5
## 관계형 데이터베이스
* SQL
* DB-API
* SQLite
* MySQL
* PostgreSQL
* SQLAlchemy
* ORM
## NoSQL
* dbm 형식
* Memcached
* Redis





