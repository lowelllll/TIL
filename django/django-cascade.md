# CASCADE
> 어떤 행위를 직접적으로 지시 받지 않았으나, 연관된 어떤 모델이 받은 행위에 같은 영향을 받게 하는 것
예를 들어, 모델 B와 모델 A가 N:1 관계에서 모델 B에 on_delete = models.CASCADE
가 설정이 되어있으면 모델 A의 어떤 레코드가 삭제되면 삭제될 모델 A의 레코드와 관련있는 모델 B의 레코드도 삭제된다.
```python
class A(models.Model):
    ...
class B(models.Model):
    a = models.ForeignKey(A,on_delete=CASCADE)
```