# 데코레이터(decorator)
> 함수를 감싸는 형태로 구성되어있으며 함수를 수정하지 않은 상태에서 추가 기능을 구현할 때 사용하는 것.

### 데코레이터 예제
```python
def trace(func):
    def wrapper():
        print("hello 실행 전")
        func()
        print("hello 실행 후")
    return wrapper

def hello():
    return "hello world!"

trace_hello = trace(hello)
trace_hello()

# hello 실행 전
# hello world!
# hello 실행 후
```
- 이처럼 함수를 직접 수정하지 않고 추가 기능을 구현한다.

### 데코레이터 생성
데코레이터를 사용할 때는 데코레이터 기능을 추가할 함수 위에 `@데코레이터` 코드를 넣어줌.
- 함수형 데코레이터
```python
def trace(func):
    def wrapper(a,b): # 매개변수 전달
        r = func(a,b)
        print("{}의 결과 -> {} + {} = {}".format(func.__name__(),a,b,r))
        return r
    return wrapper

@trace
def add(a,b):
    return a+b

add(1,2)
# add의 결과 -> 1 + 2 = 3
```

[매개변수 받는 함수형 데코레이터 예제](https://github.com/lowelllll/study-python/blob/master/python_is_my%20friends/decorator_functools_wraps.py)


- 클래스형 데코레이터
```python
class Trace:
    def __init__(self,func):
        self.func = func
    def __call__(self):
        print("{}함수 시작".format(self.func.__name__())
        func()
        print("{}함수 종료".format(self.func.__name__())

@Trace
def hello():
    print("hello world!")

hello()

```
[매개변수 받는 클래스형 데코레이터 예제](https://github.com/lowelllll/study-python/blob/master/python_is_my%20friends/decorator_class_parameter.py)

## refer
https://dojang.io/mod/page/view.php?id=1131