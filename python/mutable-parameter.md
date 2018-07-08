# 변경 가능한 기본 인자
함수 정의에서 변경 가능한 기본인자가 처리되는 방식

### 문제
```python
def append_to(element,to=[]):
    to.append(element)
    return to

my_list = append_to(12)
print(my_list)

other_list = append_to(40)
print(other_list)

# 예상한 결과
# [12]
# [40]

# 실제 결과
# [12]
# [12,40]
```

### 왜 이런 결과가 나올까?
함수가 정의될 때 새 리스트가 생성되고, 함수가 호출될 때 마다 같은 리스트가 사용됨.  
<b>파이썬의 기본 인자는 함수가 호출될 때마다 만들어지지 않고, 함수가 저의될 때만 한 번 만들어짐.</b>  
변경 가능한 기본 인자를 사용하면,매 호출마다 같은 객체를 사용하게 됨.

### 해결
함수가 호출할 때 마다 새 객체가 만들어지도록, 인자가 제공되지 않았음을 알리는 기본 인자(None)를 사용함.
```python
def append_to(element,to=None):
    if to is None:
        to = []
    to.append(element)
    return to
```

## refer
[파이썬을 여행하는 히치하이커를 위한 안내서](http://www.yes24.com/24/goods/55258117)