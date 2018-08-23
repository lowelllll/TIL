# python

1991년 귀도 반 로섬에 의해 만들어진 프로그래밍 언어.

- 다양한 OS 지원

- 풍부한 라이브러리

   웹 서비스 개발과 데이터 분석에서 강세.

- Python의 강점은 속도가 아닌 유연성.



## Python으로 할 수 있는 일

Android/iOS 네이티브 앱 개발을 제외한 모든 영역 가능

### 웹 서비스

`Django`,`Falsk`,`Pyramid` 등

### 수치연산

`Matpltlib`, `numpy`,`Pandas` 등

### GUI 프로그래밍

`PyQt`, `wxPython` 등



## Python 인터프리터 종류

인터프리터란?

> 프로그래밍 언어의 소스 코드를 바로 실행하는 컴퓨터 프로그램 또는 환경 

#### CPython

C/C++로 구현된 파이썬 인터프리터

#### IronPython

마이크로소프트 닷넷으로 구현된 파이썬 인터프리터

#### Jython

자바로 구현된 파이썬 인터프리터

#### PyPy

RPython으로 구현된 파이썬 인터프리터



인터프리터에 따라 각 구현된 언어의 라이브러리를 사용할 수 있음.



## 가상환경

> 파이썬 라이브러리를 설치할 수 있는 별도로 격리된 디렉토리

어떤 프로젝트는 python3.6 , 어떤 프로젝트는 python2.7을 사용할 때 이 환경을 서로 독립적이게 구축할 수 있음.

- 프로젝트 별로 생성된 디렉토리에 라이브러리를 격리 설치

### 가상환경 만들기

1. 새로운 가상환경 디렉토리 생성
2. 생성된 가상환경 활성화 `activate`
3. 활성된 가상환경 내에 필요한 라이브러리 설치
4. 가상환경 비활성화 `deactivate`



### venv

Python3에서 기본으로 제공하는 라이브러리



#### venv 생성

```
# window 
python -m venv [venv이름,경로]
# python -m venv ./myenv

# mac
# python3 -m venv ./myenv
```

#### 가상환경 활성화

```'
# window
myvenv/Script/activate

# mac
source myvenv/bin/activate
. myvenv/bin/activate
```

#### 가상환경 비활성화

```
deactivate
```



파이썬 2에서는 virtualenv 라이브러리 설치가 필요함.



## refer

(AskDjango)[https://www.askcompany.kr/]

