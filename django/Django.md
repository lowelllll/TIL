# Django
Python의 웹 프레임워크
## Django 기능별 특징
### MVC 기반 MTV
- 장고는 MTV(model-template-view) 패턴 기반의 프레임워크이다
- View : 데이터를 가져오고 변형하는 컴포넌트
- Template : 데이터를 사용하에게 보여주는 컴포넌트
- MVC 패턴과 개념은 동일하다
### 객체 관계 매핑(ORM)
- 객체 관계 매핑(ORM)은 데이터베이스 시스템과 데이터 모델을 연결시키는 다리와 같은 역할함.
- ORM 기능을 통해 다양한 데이터베이스 시스템을 지원
- 이미 구축한 데이터베이스 시스템을 다른 데이터베이스로 변경하는 경우에도 설정을 조금만 변경하면 됨.
### Admin 사이트
- 장고는 데이터베이스에 대한 관리 기능을 위하여 프로젝트를 시작하는 시점에 기본 기능으로 관리자 admin 페이지를 제공함.
- 관리자 화면을 통해 애플리케이션에서 사용한느 데이터들을 쉽게 생성 및 변경함.
- 개발자가 별도로 관리 기능을 개발할 필요가 없음.
### url 설계
- 우아한 url 방식 채택 -> 다른 프레임워크에서도 그대로 사용 가능
- url 형태를 개발자가 직접 결정할 수 있고, url 형태를 파이썬 함수에 직접 연결하도록 되어있어 개발이 편함.
### 자체 템플릿 시스템
- 내부적으로 확장이 가능하고 디자인이 쉬운 템플릿 시스템
- 화면 디자인과 로직에 대한 코딩을 분리하여 독립적으로 개발 진행 가능.
### 캐시 시스템
- 캐시시스템
    + 동적 페이지를 만들기 위해서 데이터베이스 쿼리,템플릿 해석, 관련 로직을 실행하여 페이지를 생성하는 것은 서버에 엄청난 부하를 주는 작업임.
    + 이것을 해결하기 위해 캐시시스템을 사용하여 자주 이용되는 내용을 저장해두었다가 재사용하는 것이 성능을 높여줌.
- 장고의 캐시시스템
    +  캐시용 페이지를 메모리,데이터베이스 내부,파일 시스템 중 아무 곳에나 저장할 수 있음.
    + 캐시 단위를 페이지에서 부터 사이트 전체,또는 특정 뷰의 결과, 템플릿의 일부 영역만을 지정하여 저장할 수 있음.
### 다국어 지원
- 동일한 소스코드를 다른나라에서도 사용할 수 있도록 텍스트 번역, 날짜/시간/숫자의 포맷, 타임존 지정 등 다국어 환경을 제공.
### 풍부한 개발 환경
- 테스트용 웹 서버를 포함하고 있어 아파치 등 상용 웹 서버가 없어도 테스트를 진행 할 수 있음.
- 디버깅 모드에서는 에러를 쉽게 파악하고 해결할 수 있도록 상세한 메시지를 보여줌.
### 소스 변경사항 자동 반영
- 장고는 *.py 파일의 변경여부를 감시하고 있다가 변경이 되면 변경내역을 바로 서버에 반영을 해줌.(웹 서버를 다시 킬 필요가 없음)

## Django 설치
- django는 파이썬 언어로 만들어졌기 때문에 파이썬이 동작하는 플랫폼에서 설치 및 사용이 가능함.

### pip를 사용하여 django 설치
- pip : python install program으로 파이썬의 오픈 소스 저장소인 pyPI(python package index)에 있는 sw 패키지를 설치하고 관리해주는 명령.
- 파이썬을 설치하면 자동으로 설치됨
cmd

    pip install Django # Django 설치

#### Django 업그레이드

    pip install Django --upgrade

#### Django 설치 확인
python shell
```python
>>> import django
>>> print django.get_version()
```

## Django 애플리케이션 개발 방식
- 웹 개발,웹 서비스 개발-> 웹 애플리케이션 개발
### 애플리케이션
- 웹 사이트를 설계할 때 가장 먼저 해야할 일 : 프로그램이 해야 할 일을 적당한 크기로 나누어 모듈화 하는 것
- 웹 사이트의 전체 프로그램 또는 모듈화 된 단위프로그램
- 프로그램으로 코딩할 대상
#### 장고에서의 애플리케이션
- 웹 사이트에 대한 전체 프로그램 : 프로젝트(Project)
- 모듈화 된 단위 프로그램 : 애플리케이션
- 애플리케이션이 모여 프로젝트를 개발하는 개념
- - -
### MVT 패턴
- MVC 패턴을 기반으로한 파이썬의 개발 패턴
- MVC?
    + Model(데이터),View(사용자 인터페이스),Controller(데이터 처리 로직)을 구분하여 한 요소가 다른 요소들에게 영향을 주지 않도록 설계하는 방식
    + ex)UI 디자이너는 데이터 관리나 애플리케이션 로직에 신경쓰지 않아도 화면 UI를 설계할 수 있고 로직이나 데이터를 설계하는 개발자도 화면 디자인은 디자이너에게 맡기고 개발 업부에 집중할 수 있게 됨.
- Model : 데이터베이스에 저장되는 데이터
- Template : 사용자에게 보여지는 부분 (MVC View에 해당)
- View : 실질적으로 프로그램 로직이 동작하여 데이터를 가져오고 적절하게 처리한 결과를 템플릿에 전달하는 역할 (MVC Controller에 해당)
#### 웹 클라이언트의 요청을 받고 장고에서 MVT 패턴에 따라 처리하는 과정
1. 클라이언트로부터 요청을 받으면 URLConf 모듈을 이용하여 URL 분석
2. URL 분석 결과를 통해 해당 URL에 대한 처리를 담당할 뷰를 분석
3. 뷰는 자신의 로직을 실행하면서 데이터베이스 처리가 필요하면 모델을 통해 처리하고 결과를 반환 받음
4. 뷰는 자신의 로직처리가 끝나면 템플릿을 사용하여 클라이언트에게 전송할 HTML 파일을 생성
5. 뷰는 최종 결과로 HTML 파일을 클라이언트에게 보내 응답함.

## Model
- 사용될 데이터에 대한 정의를 담고있는 장고의 클래스
- ORM 기법을 사용하여 애플리케이션에서 사용할 데이터베이스를 클래스로 매핑하여 코딩함
- 하나의 모델클래스는 하나의 테이블에 매핑, 모델 클래스의 속성은 테이블의 칼럼에 매핑
- 애플리케이션은 데이터베이스에 대한 액세스를 SQL 없이도 클래스를 다루는 것처럼 할 수 있어 편리함.
- 데이터베이스 엔진을 변경하더라도 ORM을 통한 API는 변경할 필요가 없어 데이터베이스 엔진을 쉽게 변경할 수 있음.
### ORM
- 객체와 관계형 데이터베이스를 연결해주는 역할
- 객체를 대상으로 필요한 작업을 실행하면 ORM이 자동으로 직접 SQL 구문이나 데이터베이스 API를 호출하여 처리해줌.

### Model 클래스 정의
```python
from django.db import models
class Person(models.Model):
	first_name = models.CharField(max_length=30)
	last_name = models.CharField(max_length=3
```
- 장고는 내부적으로 SQL 명령을 사용하여 다음과 같은 데이터베이스 테이블을 생성함
```sql
CREATE TABLE myapp_person( # 테이블명은 애플리케이션명과 클래스명을 _ 밑줄로 연결하고 모두 소문자로 표시
	"id" serial NOT NULL PRIMARY KEY,
	"first_name" varchar(30) NOT NULL,
	"last_name" varchar(30) NOT NULL
);
```
## Template
- 장고는 자체 템플릿 시스템을 가지고 있음.
- 장고에서 제공하는 템플릿은 파이썬 코드를 직접 사용할 수 있기 때문에 강력하고 확장하기 쉬운 구조로 되어있음.
- 템플릿 파일은 *.html 확장자를 가지며 템플릿 시스템 문법에 맞게 작성함.
- 템플릿 파일은 적절한 디렉토리에 위치해야함.
 장고는 settings.py의 TEMPLATE_DIRS , INSTALLED_APP에 지정된 디렉토리를 보고 템플릿 파일을 찾음
```python
INSTALLED_APPS = [ 
    'django.contrib.admin', 
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'bookmark.apps.BookmarkConfig',
    'blog.apps.BlogConfig',
]
...
'DIRS': [os.path.join(BASE_DIR,'templates')], #TEMPLTE_DIRS
```
순서대로 템플릿 디렉토리를 검색하여 템플릿 파일을 찾음.
(TEMPLATE_DIRS 항목에 정의된 디렉토리 부터 찾고 그 다음 INSTALLED_APPS 항목의 디렉토리를 찾음)
```
// 찾는 경로
project/templates
...django/contrib/admin/templates
...django/contrib/auth/templates
...django/contrib/contenttypes/templates
.
.
.
project/blog/templates
```

        