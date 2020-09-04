# 텍스트 데이터
## CSV, Table
* 읽기
``` python
pd.read_csv('파일명')
pd.read_table('파일명', sep=',')
```
* 읽기 옵션
``` python
sep/delimiter : 구분자. '\s+'로 고정되지 않은 구분자의 경우
header : None. column명으로 사용할 행 지정 
names : column명 지정
index_col : index_column 지정
skiprows : 무시할 행 지정
na_values : NA처리할 값 지정
parse_dates : 날짜를 datetime으로 변환할지 여부. [0,1]은 각 column에 적용.[[0,1]]은 각 column을 조합하여 적용
keep_data_col : datetime으로 변환시 기존 column을 남길지 여부.
converters : 변환 시 column에 적용할 함수. {'column명': 함수}
nrow : 파일의 처음 일부만 읽어올 줄 지정
iterator : 파일을 조금씩 읽을때 사용
chunksize : 한 번에 읽을 파일 크기
```
* 쓰기
``` python
data.to_csv('파일명.csv')
# 옵션
sep : 구분자 지정
na_rep : NA를 원하는 값으로 지정.
index : True. 인덱스 저장 여부
header : column명 저장 여부
```
## JSON
``` python
import json
json.loads() # 읽기
json.dumps() # 쓰기

import pandas as pd
pd.read_json()
data.to_json()
```
## XML, HTML
``` python
import lxml
from lxml import objectify
parse = objectify.parse(open('파일'))
root = parse.getroot()
for elt in root.INDICATOR:
	for child in elt.getchilder():
	...

import pandas as pd
pd.read_html
```
# 이진 데이터
## Pickle
``` python
pd.read_pickle('file_name')
```
pickle은 오래 보관할 데이터에는 적절하지 않을 수 있다.
## HDF5
대량의 과학 계산용 배열 데이터를 저장하기에 적잘한 파일 형식. 다양한 압축 기술을 활용하여 실시간 압축을 지원하며 반복되는 패턴을 가진 데이터를 효과적으로 저장한다. 대량의 데이터에서 일부 데이터만 효과적으로 읽고 쓰기에 적절한 선택지이다.
``` python
pd.read_hdf('file_name')
```
## EXCEL
``` python
pd.read_excel('filename', 'sheet')
```
# 데이터베이스
``` python
import sqlite3

import sqlalchemy as sqla
```

