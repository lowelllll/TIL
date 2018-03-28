# 웹 크롤링
웹 크롤러?
> 웹 사이트에서 원하는 정보를 가져와 내가 이용하기 편한 방식으로 가공하는 것을 도와주는 것  
웹 크롤링?
> 웹 사이트에서 원하는 정보를 자동으로 수집하는 것
### 크롤링이 쓰이는 곳
- google 검색 서비스 : 수 많은 웹 사이트를 크롤링하여 검색 서비스 제공
- 쿠차 : 각종 소셜커머스 사이트를 크롤링하여 최저가 정보를 제공
## 웹에서 정보 가져오기
### Requests
Python으로 HTTP request를 보내는 라이브러리는 urllib,mechanize,requests가 있는데 requests는 간편하게 HTTP request를 보낼 수 있고 세션 유지가 용이, python2,python3를 완벽 지원하기 때문에 requests를 사용함.
#### requests 설치
```python
pip install reqeusts
```
#### HTTP reqeust 요청
- GET 요청
```python
response = request.get('https://www.naver.com')
```
- POST 요청
```python
data = {'id':id,'pw':pw}
response = request.post('https://www.naver.com')
```
##### 요청 보내기 예제
```python
## parser.py
import requests

## HTTP GET Request
req = requests.get('https://www.naver.com')

## HTML 소스 가져오기
html = req.text
## HTTP Header 가져오기
header = req.headers
## HTTP Status 가져오기 (200: 정상)
status = req.status_code
## HTTP가 정상적으로 되었는지 (True/False)
is_ok = req.ok
```
requests 라이브러리는 html을 python이 이해하는 객체 구조로 만들어 주지 못함.  
req.text는 python의 문자열(str) 객체를 반환할 뿐이기 때문에 정보를 추출하기 어려움. -> BeautifulSoup이 해결
## 정보 구조화하기(가공)
### BeautifulSoup
html코드를 python이 이해하는 객체 구조로 변환하는 parsing을 맡고 있으며
이 라이브러리를 통해 제대로된 의미있는 정보를 추출할 수 있음.
- parsing을 도와주는 강력한 python 라이브러리
    - parsing
        > 가공되지 않은 문자열에서 필요한 부분을 추출하여 의미있는 (구조화된) 데이터로 만드는 과정
- 정규식 작성 없이 tag,id,class 등의 이름으로 쉽게 파싱 가능
#### BeautifulSoup 설치
```python
pip install bs4
```
BeautifulSoup을 직접 설치하는 것 보다 bs4라는 wrapper 라이브러리를 통해 설치하는 것이 더 쉽고 안전함.

#### BeautifulSoup의 기능
- select
    - css Selector를 이용해 조건과 일치하는 모든 객체들을 List로 반환함 (딕셔너리형태로)
#### BeautifugSoup 예시
```python
import requests
from bs4 import BeautifulSoup

# HTTP GET Request
req = requests.get('https://www.naver.com')

# HTML 소스 가져오기
html = req.text

# BS로 html 소스를 python 객체로 변환
# 첫 인자는 html 소스코드, 두번째 인자는 어떤 parser를 이용할 지 명시함.
## python 내장 html.parser를 이용함

soup = BeautifulSoup(html,'html.parser')
# soup 객체에서 원하는 정보를 찾아낼 수 있음.
# soup 객체 태그로 구성된 요소를 python이 이해하는 상태로 바꾼 것

# Css Selector를 통해 html 요소를 찾아냄.
my_titles = soup.select(
    'h3 > a'
)

# my_titles : soup 객체들의 list , 태그의 속성도 사용할 수 있음.

# title 객체는 dictionary와 같이 태그의 속성들을 저장가능함.
for title in my_titles :
    # tag안의 텍스트
    print(title.text)
    #Tag의 속성을 가져오기
    print(title.get('href'))
    # title['href']도 가능
```
## Scrapy
- 링크를 타고 다른 페이지로 이동하며 scrapping하는 web spider 제작에 용이
    - web spider 
        > 인터넷을 크롤링하며 정보를 수집하고 필터링하며 사용자를 위한 정보를 한데 모으는 소프트웨어 에이전트, 특정 목적을 위해 특정한 방법으로 인터넷을 크롤링하는 프로그램으로 정보를 수집하거나 웹 사이트의 구조와 유효성을 파악하는 것 
        - 검색 엔진의 기초가 됨. 
        - 웹에서 자동으로 데이터를 검색하여 검색어에 가장 잘 맞는 웹 사이트의 내용을 인덱싱하는 다른 애플리케이션에 전달함.

## refer
https://beomi.github.io/gb-crawling/posts/2017-01-20-HowToMakeWebCrawler.html  
http://www.test104.com/kr/tech/2760.html  
https://www.slideshare.net/2minchul/web-scraping-75314593