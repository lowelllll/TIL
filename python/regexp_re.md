# Python 정규표현식

## 정규표현식

> RegExp ,  문자열의 패턴,규칙.rule을 정의하는 방법

- 문자열 검색이나 치환 작업을 편하게 할 수 있음.

### 1글자 패턴

- 숫자 1글자 

  `[0-9]`, `\d` 

- 알파벳 소문자 1글자 

  `[a-z]`

- 알파벳 대문자 1글자

  `[A-Z]`

- 16진수 1글자 

  `[0-9a-fA-F]`

- 문자열의 시작을 지정

  `^`

- 문자열의 끝을 지정 

  `$`

## re

> 파이썬3 정규표현식 라이브러리

#### 사용법

```python
import re
if re.match('[가-힣]{3}','이예진'): # match function re.match(pattern,string)
    print "match"
else:
    print "not match"
```

#### escaping

이스케이핑을 해야하는 문자열 앞에 `r`을 붙여주면 자동 이스케이핑 처리가 된다.

```python
pattern = "^01[0-9][1-9]\\d{6,7}$"

raw_pattern =  r"^01[0-9][1-9]\d{6,7}$"
```

