# Pagination 
### 페이징(Pagination)
> 화면에 보여줄 데이터가 많은 경우 한 페이지 분량에 맞게 적절한 크기로 나눠서 페이지 별로 보여주는 기능

## Pagination 클래스
> 페이징 기능의 메인 클래스,객체의 리스트와 페이지 당 항목 수를 필수 인자로 받아 각 페이지 객체를 생성해줌.

```python
class Paginator(object_list,per_page)
```
### Arguments 
- object_list : 페이징 대상이 되는 객체 리스트이며 리스트,튜플,QuerySet 객체 가능함 (count(),__len__() 메소드를 가지고 있는 객체여야 함)
- per_page : 페이지 당 최대 항목 수

### Method 
#### Paginator.page(number)
> Page 객체를 Paginator를 통해 생성할 수 있도록 제공함.

number:페이지 번호
get_page와 동일한 기능을 함.

### Attribute
#### Paginator.count 
항목의 총 개수
#### Paginator.num_pages 
페이지의 총 개수
#### Paginator.page_range
1부터 시작하는 페이지 범위 ex) [1,2,3,4] or range(1,4)

## Page 클래스
> Paginator 객체에 의해 생성된 단위 페이지를 나타내는 객체  

한 페이지의 정보를 가지고 있다고 보면 됨. 페이지의 항목,이전 페이지 정보, 이후 페이지 정보 등등..

```python
class Page(object_list,number,paginator)
```
보통 이렇게 안하고 Paginator.page() 메소드로 객체를 생성한다.

### Arguments 
- object_list : Paginator와 동일
- number : 몇 번째 페이지인지 지정하는 페이지 인덱스
- Paginator : 페이지를 생성 해주는 Paginator 객체

### Method 
#### Page.has_next()
다음 페이지가 있으면 True 반환
#### Page.has_previous()
이전 페이지가 있으면 True 반환
#### Page.has_other_page()
다음이나 이전 페이지가 있으면 True 반환
#### Page.next_page_number()
다음 페이지의 번호 반환, 없으면 오류 (현재 2페이지, 다음 페이지 3 반환,없을 시 InvalidPage 익셉션)
#### Page.previous_page_number()
이전 페이지의 번호 반환, 없으면 오류 (현재 2 페이지, 이전 페이지 1 반환, 없을 시 InvalidPage 익셉션)
#### Page.start_index 
해당 페이지의 첫 번째 항목의 인덱스 반환 (1부터 카운트, 5개 항목 중 페이지 당 2개의 항목 씩 포함된다면 두 번째 페이지의 start_index = 3)
### Page.end_index
해당 페이지의 마지막 항목의 인덱스 반환 (1부터 카운트, 5개 항목 중 페이지 당 2개의 항목 씩 포함된다면 두 번째 페이지의 end_index = 4)

### Attribute
#### Page.object_list
현재 페이지의 객체 리스트
#### Page.number 
현재 페이지의 번호
#### Page.paginator
Paginator 객체

## Paginator 예문 - 1 
```python
>>> from django.core.paginator import Paginator
>>> objects = ['john','paul','george','ringo']
>>> p = Paginator(objects,2)
>>> p.count # 항목 개수
4
>>> p.num_pages # 페이지 개수
2
>>> p.page_range # 페이지 범위
range(1, 3)
>>> page1 = p.page(1) # Page 객체 생성
>>> page1
<Page 1 of 2>
>>> page1.object_list # Page 객체의 object 항목들 
['john', 'paul']
>>> page2 = p.page(2)
>>> page2.object_list
['george', 'ringo']
>>> page2.has_next() # next 페이지 여부 확인
False
>>> page2.has_previous() # previous 페이지 여부 확인
True
>>> page2.previous_page_number() # previous 페이지 번호 
1
>>> page2.start_index() #  현재 페이지의 시작 항목 번호
3
>>> page2.end_index() # 현재 페이지의 마지막 항목 번호
4
>>> page2.next_page_number() # next 페이지 번호 확인 -> 없으면 오류
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "C:\Users\..\lib\site-packages\django\core\paginator.py", line 162
, in next_page_number
    return self.paginator.validate_number(self.number + 1)
  File "C:\Users\..\lib\site-packages\django\core\paginator.py", line 47,
 in validate_number
    raise EmptyPage(_('That page contains no results'))
django.core.paginator.EmptyPage: That page contains no results
>>> page1.next_page_number()
2
>>> page2.previous_page_number()
1
```
## Paginator 예문 -2
```python
>>> from django.core.paginator import Paginator
>>> from post.models import Post,Reple
>>> post = Post.objects.get(pk=47)
>>> reple_list = Reple.objects.filter(post=post)
>>> reple_list
<QuerySet [<Reple: test1>, <Reple: test2>, <Reple: test3>, <Reple: test4>, <Reple: test5>, <Reple:
test6>, <Reple: test7>, <Reple: test8>]>
>>> p = Paginator(reple_list,3)
<string>:1: UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered
 object_list: <class 'post.models.Reple'> QuerySet.
>>> p.get_page(1) # page 얻기
<Page 1 of 3>
>>> p.get_page(2)
<Page 2 of 3>
>>> p.get_page(3)
<Page 3 of 3>
>>> p.page(3)
<Page 3 of 3>
>>> page3 = p.page(3)
>>> page3
<Page 3 of 3>
>>> page3.object_list # 페이지 항목
<QuerySet [<Reple: test7>, <Reple: test8>]>

```

# refer
[파이썬 웹 프로그래밍](http://www.hanbit.co.kr/media/community/review_view.html?hbr_idx=3231)  
https://docs.djangoproject.com/en/2.0/topics/pagination/