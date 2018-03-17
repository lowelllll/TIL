# Django Ajax
## 장고에서 Ajax data 받기
ajax에서 data 키로 보낸 값은 django view에서 http method 변수로 받을 수 있다.
```python
def ajax_receive(request):
    data = request.GET.get('data key')
```
## Ajax 오류 해결
### Object of type 'QuerySet' is not JSON serializable
이 오류는 객체를 구한 뒤, JsonResponse를 할 때 그냥 객체 그대로를 data로 보낼 시 오류가 난다. 
```python
    users = User.objects.filter(
        Q(username__icontains=username)
    )
    data = {
        'users':users
    }
    return JsonResponse(data) # TypeError: Object of type 'QuerySet' is not JSON serializable
```
해결:QuerysSet Api values를 사용하여 해결
> valuse() : QuerySetiterable로 사용될 경우 모델 인스턴스가 아닌 사전을 반환합니다.
```python
    users = User.objects.filter(
          Q(username__icontains=username)
    ).values()
    return JsonResponse({"models_to_return": list(queryset)})
```
## refer
https://stackoverflow.com/questions/16790375/django-object-is-not-json-serializable  
https://simpleisbetterthancomplex.com/tutorial/2016/08/29/how-to-work-with-ajax-request-with-django.html