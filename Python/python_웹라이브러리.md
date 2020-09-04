# 파이썬 표준 웹 라이브러리

* http : 모든 클라이언트-서버 HTTP 세부사항을 관리한다.
  * client - 모든 클라이언트 부분을 관리
  * server - 파이썬 웹 서버 작성에 도움
  * cookies/cookiejar - 쿠키 관리
* urllib : http 위에서 실행
  * request - 클라이언트의 요청을 처리
  * response - 서버의 응답을 처리
  * parse - URL을 분석



아래 코드를 통해 TCP/IP 커넥션을 열고, HTTP 요청을 만들고 응답을 받을 수 있다.
``` python
from urllib import request

url = "http://URL주소"
conn = requeset.urlopen(url)
data = conn.read()
```
* HTTP 상태 코드
1xx : 조건부 응답
  	서버는 요청을 받았지만, 클라이언트에 대한 몇 가지 추가정보가 필요
2xx : 성공
  	성공적으로 요청을 처리
3xx : 리다이렉션
  	리소스가 이전되어 클라이언트에 새로운 URL을 응답해 줌
4xx : 클라이언트 에러
  	클라이언트 측에 문제가 있음
5xx : 서버 에러
  	서버에 문제가 있음.


# Requests 라이브러리
서드파티의 requests 모듈을 사용하면 코드가  좀 더 짧고 이해하기 쉽다.
[Requests 라이브러리 문서](http://docs.python-requests.org/)

``` python
import requests

url = "http://URL주소"
resp = requests.get(url)
>>> resp.text
```

