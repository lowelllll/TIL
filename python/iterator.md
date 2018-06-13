# 이터레이터 (Iterator)
>  `__next__()` 메소드를  사용해 차례대로 값을 꺼낼 수 있는 객체.

- 반복 가능한 객체(iterable)에서 `__iter__()` 메소드로 이터레이터를 얻음.
 
## iterable 
> 반복 가능한 객체

- ex) 문자열,리스트,딕셔너리,세트 등
- 객체에 `__iter__()` 메소드가 있음.

```python
# str iterator 생성
>>> it = "hello".__iter__()
>>> type(it)
<class 'str_iterator'>
>>> it.__next__()
'h'
>>> it.__next__()
'e'
>>> it.__next__()
'l'
>>> it.__next__()
'l'
>>> it.__next__()
'o'

# range iterator 생성
>>> it = range(3).__iter__()
>>> type(it)
<class 'range_iterator'>
>>> it.__next__()
0
>>> it.__next__()
1
>>> it.__next__()
2
>>> it.__next__()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration # iterator가 끝나면 StopIteration exception 을 발생
```

`range()` 는 숫자를 모두 미리 만드는 것이 아닌 연속된 값을 차례대로 꺼낼 수 있는 이터레이터를 만듬.  
반복할 때 마다 이터레이터에서 숫자를 하나씩 꺼내 반복함.  
연속한 숫자를 미리 만들면 메모리를 많이 사용하기 때문에 이터레이터만 생성하고 값이 필요한 시점에 값을 만듬.