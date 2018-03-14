# User Model Custom하기
django에서 제공하는 auth.User model을 custom 하는 방법이다. 
custom 하는 방법은 User모델과 1:1 연결하는 방법과 AbstractUser를 통해 User모델을 객체하는 방법이 있다.  
- 1:1 연결  
이 방법은 자신이 별도로 정의한 테이블과 User 모델이 1:1 연결이 되었으면 할 때,
User와 관련 정보를 저장하면서 User 모델 자체를 유지하고 싶을 때 사용한다.
- AbstractUser 사용  
직접적으로 User 모델에 필드를 추가하고 싶을때 사용한다. 

## OneToOne 필드 사용하여 user model custom
### 1.1 accounts app 생성
```
$ python manage.py startapp accounts
```
앱은 꼭 settings.py에 등록한다.
### 2.1 models.py 설정
```python
from django.conf import settings
from django.db.models.signals import post_save
from django.dispatch import receiver

class Profile(models.Model):
    user = models.OneToOneField(settings.AUTH_USER_MODEL,on_delete=models.CASCADE)
    avatar = models.ImageField(upload_to='profile/%Y/%m')

@receiver(post_save, sender=settings.AUTH_USER_MODEL)
def create_user_profile(sender, instance, created, **kwargs):
    if created:
        Profile.objects.create(user=instance)

@receiver(post_save, sender=settings.AUTH_USER_MODEL)
def save_user_profile(sender, instance, **kwargs):
    instance.profile.save()
```
- makemigration , migrate 실행
### 2.2 forms.py 설정
```python
from accounts.models import Profile

class ProfileForm(forms.ModelForm):
    class Meta:
        model = Profile
        fields = ('avatar',)
```
### 2.3 views.py 설정
```python
def profile_update(request):
    if request.method=='POST':
        form = ProfileForm(request.POST,request.FILES,instance=request.user.profile) # instance form의 save()에서 실행됨
        if form.is_valid():
            form.save()
            return redirect('post:post_list')
    else:
        form = ProfileForm(instance=request.user.profile)
    return render(request,'post/profile_form.html',context={'form':form})
```
### 2.4 template 설정
```html
{% if user.profile.avatar %} 
    <img src="{{ user.profile.avatar.url }}" alt="avatar">
{% endif %}
```
### 3.1 Profile 모델 접근
```python
>>> u = User.objects.get(pk=1)
>>> u.profile 
<Profile: Profile object (1)>
>>> u.profile.avatar
<ImageFieldFile: profile/2018/03/20837007_261679467670462_1377536040645951488_n.jpg>
```
## refer 
https://simpleisbetterthancomplex.com/tutorial/2016/07/22/how-to-extend-django-user-model.html
http://whatisthenext.tistory.com/118  
https://recall2300.github.io/2017-01-11/extendsusermodel/
https://stackoverflow.com/questions/44109/extending-the-user-model-with-custom-fields-in-django