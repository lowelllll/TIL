# MySQL 
> MySQL은 세계에서 가장 많이 쓰이는 오픈 소스의 관계형 데이터베이스 관리 시스템이다.  
## MySQL 사용하기
### MySQL 관리자 접속
권리자로 MySQL을 접속한다. 
-p 명령어는 패스워드를 입력하게 하는데, 생략해도 된다.
```
> mysql -u root -p 
```
### Database 조회
```
> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
```
### Database 사용
use 명령어는 해당 데이터베이스를 사용하겠다는 의미이다.  
```
> use databasename;
# use mysql;
```

### Database 생성
명령어는 되도록 대문자로 작성함.
```
> CREATE DATABASE db;
```

### table 조회
```
> show tables;
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| column_stats              |
| columns_priv              |
| db                        |
| ...                       |
| time_zone_transition_type |
| user                      |
+---------------------------+
```
### table 확인
table 필드,필드 속성 조회
```
> desc user;
+------------------------+-----------------------------------+------+-----+----------+-------+
| Field                  | Type                              | Null | Key | Default  | Extra |
+------------------------+-----------------------------------+------+-----+----------+-------+
| Host                   | char(60)                          | NO   | PRI |          |       |
| User                   | char(80)                          | NO   | PRI |          |       |
| Password               | char(41)                          | NO   |     |          |       |
| ......                 |                                   |      |     |          |       |
+------------------------+-----------------------------------+------+-----+----------+-------+
```




