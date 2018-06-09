# Selenium
> 웹 애플리케이션을 위한 테스팅 프레임 워크

- 자동화 테스트를 위한 여러가지 강력한 기능을 제공.
- 다양한 브라우저 지원, 다양한 테스트 작성 언어 지원(java,python,php 등)
- `webdriver`라는 API를 통해 운영체제에 설치된 브라우저를 제어할 수 잇음.
    - 자바스크립트를 이용해 비동기적으로 혹은 뒤늦게 불러와지는 컨텐츠들을 가져올 수 있음.
    - 실제 브라우저가 동작하기 때문에 JS로 렌더링이 완료된 후 DOM 결과물에 접근이 가능함.

## python에서 Selenium 사용하기
1. selenium 설치
```python
pip install selenium
```

2. chrome 드라이버 다운로드
https://sites.google.com/a/chromium.org/chromedriver/downloads

- 추가로 운영체제에서는 크롬이 깔려져 있어야 하며 최신 버전인지 확인하고 
최신 버전이 아니라면 업데이트.
- chrome 브라우저 뿐만 아니라 firefox 등 다른 브라우저도 가능.

3. 사이트 브라우징
```python
from selenium import webdriver
driver = webdriver.Chrome('<다운로드 받은 드라이버의 경로>')

driver.implicitly_wait(3) # 웹 자원 로드를 위해 3초간 기다림

driver.get('https://google.com') # url에 접근.

```

## refer
https://beomi.github.io/2017/02/27/HowToMakeWebCrawler-With-Selenium/

http://wiki.gurubee.net/pages/viewpage.action?pageId=6259762