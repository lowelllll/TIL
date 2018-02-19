# django shortcut function

### render (request,template,context …)
템플릿과 컨텍스트를 합쳐 HttpResponse 객체를 리턴함. 
```python
def hello(request):
    return render(request,'index.html',{'foo':'bar'})
```
아래 예는 위 코드 결과와 같다.
```python	
def view2(request):
    template = get_template('index.html')
    context= {'foo':'bar'}
    return HttpResponse(template.render(context,request))
```
### redirect(to, permanent=False, *args, **kwargs) 
url 주소로 써주거나 url name을 쓰면 redirect 내부에서 자동으로 reverse를 호출하여 이름을 매칭하여 이동.
```python
def view1(request):
    redirect('/url/')
	return redirect('index.html',foo='bar') 
```
### get_object_or_404(model)
모델의 데이터를 가져올때 해당 모델을 가져오지 못한 경우 Http404를 raise 한다.
```python
def view3(request):
    object = get_object_or_404(Model)
```
# django.core.urlresolvers utility functions
### reverse(viewname,urlconf,*args,**kwargs) 
- url 패턴으로 부터 url 스트링을 구할 수 있어 url 스트링을 하드코딩 하지 않도록 해줌.
```python
reverse('admin:app_list', args,kwargs={'app_label': 'auth'})
#/admin/auth/ url 스트링
```
# HttpResponse,HttpResponseReirect 차이

#### HttpResponse 
- http 코드가  200 이고 생성자로 전달된 컨텐츠를 포함한 HttpResponse오브젝트를 만든다.
- 작은 response에서 사용 (ajax 데이터 ,작은 number)
#### HttpResponseRedirect
- http가 302인 HttpResponse 객체를 만듬. 다른 페이지로 redirect할때만 써야함 


## 참고
[[Django] HttpResponse VS HttpResponseRedirect](https://milooy.wordpress.com/2016/03/03/django-httpresponse-vs-httpresponseredirect/)
