# User Model Custom하기
### OnetoOne field 사용
```python
@python_2_unicode_compatible
class Profile(models.Model):
    user = models.OneToOneField(User,on_delete=models.CASCADE)
    profile_img = models.ImageField(upload_to='profile/%Y/%m') # 새로 추가될 field

@receiver(post_save,sender=User)
def create_user_profile(sender,instance,created,**kwargs):
    if created:
        Profile.objects.create(user=instance)

@receiver(post_save,sender=User)
def save_user_profile(sender,instance,**kwargs):
    instance.profile.save()
```
## refer 
https://simpleisbetterthancomplex.com/tutorial/2016/07/22/how-to-extend-django-user-model.html