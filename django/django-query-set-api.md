# Django QuerySet Api
### exists()
모델의 조건에 맞는 객체가 있는지 확인 후 , 있으면 True, 없으면 False 값을 리턴
```python
if Post.objects.filter(pk=1).exists():
    ...
```
### count()
모델의 조건에 맞는 객체의 개수를 리턴함.
```python
post_count = Post.objects.filter(pk=1).count()
```
