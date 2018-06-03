# with 구문
> with 구문은 context manager에 의해 실행되는 `__enter()__` 과 `__exit()__`를 정의해  with 구문 body의 앞 부분과 뒷 부분에 실행되는 코드를 대신할 수 있음.

with 구문을 이용하면 try/finally 를 대신해 더 편하고 쉽게 사용할 수 있음.

```python
set things up
try:
    do sometings
finally:
    tear things down
```

set things up - file을 열거나 외부 리소스 처리

tear things down - file을 닫고 리소스 제거,해제 처리 무조건 실행되는 것을 보장함.

이러한 코드는 많이 사용되므로 set things up, tear things down 부분을 메소드로 정의해 사용하면 재사용성이 높아 질 것임.

```python
class controlled_execution:
    def __enter__(self):
        set things up
        return thing
    def __exit__(self, type, value, traceback):
        tear things down

with controlled_execution() as thing:
     some code using thing
```
- with 구문이 실행되면 context manager에 의해 `__enter()__` 이 실행되고 여기서 반환되는 값이 as의 thing으로 지정됨.  
그 후 some code using thing 코드가 실행되고 마지막에는 코드에 무슨 일이 있다 해도 `__exit()__` 호출은 보장됨.

```python
with open("x.txt") as f:
    data = f.read()
    do something
```
- file 객체는 `__enter()__` , `__exit()__` 메서드가 구현되어 있으므로 `with`을 사용해여 간단하게 파일을 열고 닫을 수 있음.

## refer
https://cjh5414.github.io/python-with/