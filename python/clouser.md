# Closure
## 변수의 사용범위
#### 전역범수
> 함수를 포함하여 스크립트 전체에서 접근할 수 있는 변수

#### 전역 범위 (global scope)
> 전역 변수에 접근할 수 있는 범위 

```python
foo = 5
def bar():
    print(foo)

bar() # 5
```
전역 변수를 사용하는 것은 전역 범위 내에서 가능하다.

#### 지역 변수
> 변수를 만든 함수 안에서만 접근할 수 있고 함수 바깥에서는 접근할 수 없는 변수.

#### 지역 범위
> 지역 변수를 접근할 수 있는 범위

```python
foo = 5
def bar():
     foo += 20
print(foo)
```

이 코드는 실행하면 오류가 난다.  
함수 안에서 전역 변수 foo 를 사용하기 위해 코드를 작성한 것이나 파이썬은 bar 함수 내 지역 변수 foo 를 만들게 된다.  
파이썬은 지역 변수 foo에 20을 더하는데, foo변수가 미리 선언되어있지 않으므로 오류가 나게 된다.

이 코드를 정상적으로 실행하기 위해선 <b>global</b> 키워드를 사용한다. 
(함수에 매개변수에 전역 변수를 전달해도 됨)

```python
foo = 5
def bar():
    global foo # 전역 변수 foo 사용
    foo += 20
print(foo)
```

전역 변수가 없을 때 함수 안에서 global 키워드를 사용하면 해당 변수는 전역변수가 된다.

함수 바깥에 있는 지역 변수의 값을 변경하기 위해선 <b>nonlocal</b> 키워드를 사용한다.

```python
def foo():
    i = 0
    def bar():
        # i += 1 error
        nonlocal i # foo의 지역 변수 i를 사용함
        i += 1
    bar()
    print(i) # 1
```

## 클로저
> 함수를 둘러싼 환경(지역 변수,코드 등)을 계속 유지하다가 함수를 호출할 때 다시 꺼내서 사용하는 함수

- 프로그램의 흐름을 변수에 저장할 수 있음.
- 내부 데이터를 숨기고 싶을 때 사용함.

```python
def calc():
    a = 3
    b = 5
    def mul_add(x):
        return a*x+b
    return mul_add

c = calc # mul_add 함수를 반환
print(c(1),c(2))
```