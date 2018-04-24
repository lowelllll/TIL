# Django QuerySet Api
### exists()
> 모델의 조건에 맞는 객체가 있는지 확인 후 , 있으면 True, 없으면 False 값을 리턴
```python
if Post.objects.filter(pk=1).exists():
    ...
```
### count()
> 모델의 조건에 맞는 객체의 개수를 리턴함.
```python
post_count = Post.objects.filter(pk=1).count()
```

### __in
> 주어진 리스트 안에 존재하는 자료 검색 (iterable Type : list,tuple,queryset)
```python
inner_qs = Blog.objects.filter(name__contains='Cheddar')
entries = Entry.objects.filter(blog__in=inner_qs) # blog가 innser_qs의 객체인 자료 검색
```
