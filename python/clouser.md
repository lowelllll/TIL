# 클로저
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