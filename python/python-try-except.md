# 파이썬의 예외처리
파이썬에서의 예외처리는 try,except 구문을 사용하여 한다.
```python
try:
    ...
except:
    ...
```
try 블록 수행 중 오류가 발생하면 except 블록이 수행되고 try 블록에서 오류가 발생하지 않으면 except 블록은 수행되지 않음.  
### except 구문은 3가지 방법으로 사용가능
- except만 사용
```python
try:
    ...
except:
    ...
```
- 발생 오류를 포함한 except문 사용
미리 정해 놓은 오류 이름과 일치할 때만 except 블록을 수행함.
```python
try:
    ...
except SyntaxError: # 문법 오류시 except 블록 수행
    ...
```
- 발생 오류와 오류 메시지 변수를 포함한 except문 사용
두번째 경우에서 오류 메시지의 내용까지 알고 싶을 경우 사용
```python
try:
    ...
except SyntaxError as e: # python2: SyntaxError,e
    print(e)
```

### try-else
else 절은 예외가 발생하지 않은 경우 실행하며 except절 바로 다음에 위치해야함.
```python
try:
    ...
except:
    ...
else: # try 구문에서 오류가 나지 않을 시 실행.
    ...
```

### try-finally
try문 수행 중 예외 발생 여부에 상관 없이 실행되는 구문,보통 사용한 리소스를 close해야 할 경우 많이 사용함.
```python
try:
    ...
except SyntaxError:
    ...
finally:
    ...
```
## refer 
https://wikidocs.net/30