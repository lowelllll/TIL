# Relation QuerySet 성능 높이기

장고에서 ORM을 사용해 데이터를 불러올 때 성능을 높이기 위해 사용하는 `select_related`, `prefetch_related`

JOIN을 통해 쿼리 갯수를 줄여 조회 쿼리 성능을 향상.

- select_related
  - Foreign Key,OneToOne 관계에서 사용
  - Foreign Key - Post(1):Comment(N) 관계에서 Comment에 해당.
- prefetch_related
  - ManyToMany, Foreign Key reverse 관계에서  사용.
  - Foreign Key reverse - Post(1):Comment(N) 관계에서Post에 해당.

### select_related()

```django
Comment.objects.all().select_related('post')
```

### prefetch_related()

```django
Post.objects.all().prefetch_related('tag_set')
```

