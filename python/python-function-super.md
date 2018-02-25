# super()
- python의 클래스는 상속 기능을 제공하는데 super() 함수는 슈퍼클래스, 즉 상속 받은 클래스의 메소드를 호출하라는 의미의 함수이다.
```python
class A():
    def __init__(self):
        print("class A __init__()")

class B(A):
    def __init__(self):
        print("class B __init()__()")
        super(B,self).__init__() # 상속받은 부모 클래스의 __init__ 메소드 호출

b = B()
```
출력 결과
```
class B __init__()
class A __init__()
```
- 만약 다중상속(슈퍼클래스가 여러개)일 시 __mro__(Method Resolution Order,메서드 탐색 순서)로 클래스 호출 순서가 결정된다.
```python
class A():
    def __init__(self):
        print("class A __init__()") # 자식 클래스에서 super 함수로 호출된 최상단 부모클래스 A클래스는 한번 호출된다.

class B(A):
    def __init__(self):
        print("class B __init__()")
        super(B,self).__init__()

class C(A):
    def __init__(self):
        print("class C __init__()")
        super(C,self).__init__()

class D(B,C):
    def __init__(self):
        print("class D __init__()")
        super(D,self).__init__()

print(D.__mro__) 
d = D()
```
위 코드는 다음과 같이 출력된다.
```
(<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
class D __init__()
class B __init__()
class C __init__()
class A __init__()
```
mro의 순서와 동일하게 __init__() 메소드가 호출된 것을 확인할 수 있다.

### 응용하기
```python
class Parents():
    def __init__(self,child):
        print("%s의 부모님" % child)

class Child(Parents):
    def __init__(self):
        self.name = 'lowell'
        print("나는 %s" % self.name)
        super(Child,self).__init__(self.name)


C = Child()
```
출력
```
나는 lowell
lowell의 부모님
```

## refer
[python super() 함수와 2.X 와 3.X 사용법](http://bluese05.tistory.com/5)