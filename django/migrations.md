# Migrations

> 모델의 내용이 변경이 되면 데이터베이스 테이블에 반영해주는 것

- 모델 변경내역 히스토리 관리
- 모델의 변경 내역을 database schema로 반영시키는 효율적인 방법을 제공함.

### makemigrations 

```django
python manage.py makemigrations [appname]
```

- 모델 변경 내역을 마이그레이션 파일 생성.

### migrate

```django
python manage.py migrate [appname]
```

- 해당 마이그레이션 파일을 실제 DB에 반영.

#### showmigrations 

```
python manage.py showmigrations [appname]
```

- 마이그레이션 기록을 확인.

#### sqlmigrations

```
python manage.py sqlmigrations [appname] [migration-name]
```

- 지정된 마이그레이션의 SQL 내역.



#### migrate farward / backward

마이그레이트 실행/마이그레이트 취소

```
python manage.py migrate [app이름] zero # 마이그레이트 전부 취소
python manage.py migrate [app이름] 0007 
# 0006이 마이그레이트 되었을 때 , 0007 마이그레이트 실행 이미 0007이 마이그레이트 되었다면 마이그레이트 취소.
```



