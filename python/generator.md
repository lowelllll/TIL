# 제너레이터 (generator)
> 이터레이터를 생성해주는 함수,발생자(이터레이터의 특수한 형태.)

- 제너레이터 함수:함수 안에서 `yield`를 사용하여 데이터를 하나씩 리턴하는 함수.
- 제너레이터 함수를 호출하면 제너레이터 객체가 반환.
- 함수를 끝내지 않은 상태에서 `yield`를 사용해 값을 바깥으로 전달할 수 있음.
    - `__next__` 메서드를 호출할 때 마다 함수안의 `yield` 까지 코드를 실행하고 `yield` 값을 발생시킴.
    - yield로 값을 반환 후, 코드 실행을 함수 바깥에 양보함.(함수가 끝나는게 아님)
- 리스트나 set과 같은 iterator는 해당 컬렉션이 이미 모든 값을 가지고 있는 경우이나, Generator는 모든 데이터를 가지고 있지 않은 상태에서 `yield`에 의해 하나씩만 데이터를 만들어 가져온다는 차이점이 있음.

### 예제
```python
#  Generator 함수
def gen():
    yield 1
    yield 2
    yield 3

g = gen()
print(type(g)) # <class 'generator'>

# next()함수로 yield 까지 코드를 실행.
n = next(g); print(n); # 1
n = next(g); print(n); # 2
n = next(g); print(n); # 3

# for문 사용하기
for x in gen():
    print(x)

```

