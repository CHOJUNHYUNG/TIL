# 크롤링 & 스크래핑

대표적인 모듈
* scrapy : 이는 모듈이 아닌 프레임워크로 기능이 많지만 설정이 복잡하다.
* BeautifulSoup : HTML로부터 데이터를 추출할때 좋은 모듈이다.

# urllib
## urlretrieve :  파일로 저장하기
``` python
# url과 저장경로 지정
url = "https://www.multicampus.com/img/saas/main/logo/CUS0001/pc_main.png"
savename = "test.png"

# 다운로드 - urlretrieve   # 다운로드 -> 파일로 저장
import urllib.request

urllib.request.urlretrieve(url,savename) # (다운받아 올 url, local에 다운 받을 경로)
```
## urlopen : 읽어오기
``` python
mem = urllib.request.urlopen(url).read() # 메모리에 적재
savename = "test2.png"

with open(savename,mode="wb") as f: # 파일로 저장
    f.write(mem)
```

# BeautifulSoup
``` python
from bs4 import BeautifulSoup
```
## BeautifulSoup 객체 생성
``` python
import urllib.request as req
from bs4 import BeautifulSoup

url = "https://finance.naver.com/marketindex/exchangeDetail.nhn?marketindexCd=FX_USDKRW"
res = req.urlopen(url)
soup = BeautifulSoup(res,'html.parser') # 객체 생성 : (변수(문서) , 분석기 종류)
```
* html 태그 접근하기
``` python
p = soup.select_one("#content > div.spot > div.today > p.no_today").text
print(p) # 1,156.50

h1 = soup.html.body.h1
p1 = soup.html.body.p # 가장 처음으로 만나는 p태그만 추출
p2 = p1.next_sibling.next_sibling # 형제 태그(동등한 수준) 추출 
print(h1.string) # 안쪽 텍스트 출력
```
## find / find_all
``` python
# find 함수 : tag, id를 이용하여 직접 접근
soup = BeautifulSoup(res,'html.parser') 

title = soup.find(id = "title")
body = soup.find(id = "body")
print(title.string)
```
``` python
# find_all() : 여러개의 태그를 한번에 추출 # find_all('tag')
# attrs[속성명] : '속성명'에 해당하는 값 반환
soup = BeautifulSoup(res,"html.parser")
links = soup.find_all("a") # type : list

for a in links:
  href = a.attrs['href']
  text = a.string
  print(text,"->",href)
```

## 로그인이 필요한 스크래핑

``` python
import requests
from bs4 import BeautifulSoup

USER = "아이디"     # 민감한 정보는 환경변수에 따로 저장하는 것이 좋음.
PASS = "비밀번호"   # os.environ['PASS']

# 세션(server-client간 연결) 시작하기 
session = requests.session()
# 로그인
login_info = {"m_id":USER , "m_passwd":PASS}
url_login = "https://www.hanbit.co.kr/member/login_proc.php"
res = session.post(url_login, data=login_info)

# 페이지 이동. get()
res = session.get("http://www.hanbit.co.kr/myhanbit/myhanbit.html")

# 스크래핑
soup = BeautifulSoup(res.text,'html.parser')
mileage = soup.select_one("#container > div > div.sm_mymileage > dl.mileage_section1 > dd > span").get_text()
ecoin = soup.select_one("#container > div > div.sm_mymileage > dl.mileage_section2 > dd > span").get_text()
print("마일리지: "+mileage+"원")
print("e-coin: "+ecoin+"원")
```

## 웹 브라우저를 이용한 스크래핑

``` python
# 웹 브라우저를 이용한 스크래핑
# Selenium : 웹 브라우저를 조작(스크래핑) 도구
# Phantomjs : 화면 없이 명령줄에서 이용할 수 있는 웹 브라우저
# Phantomjs를 이용하여 사이트 탐색(이동), Selenium을 이용하여 해당 사이트에서 자동으로 자동으로 url 열기, 클릭, 스크롤, 문자 입력 가능, 화면 캡쳐 등 작업 수행
```

``` python
# 캡쳐 파일 크롤링
from selenium import webdriver
url='https://www.naver.com/'

# phantomjs 드라이버 추출
browser=webdriver.PhantomJS()
browser.implicitly_wait(3) # 3초 대기

# url 읽기
browser.get(url)

# 화면 캡쳐
browser.save_screenshot('myshot.png')

# 브라우저 종료
browser.quit()
```





``` python
# cf) HTML과 XML
# https://www.weather.go.kr/weather/forecast/mid-term-rss3.jsp
    #HTML은 가독성이 떨어짐. 형식,구조가 없기때문에(비구조적 문서) 컴퓨터가 이해하기 어려움
    #XML HTML의 대안으로 나옴. 구조적 문서 => 위계적 구조
    #최근에는 json을 많이 사용. 딕셔너리 형식의 파일. 장점: 가벼움
```

