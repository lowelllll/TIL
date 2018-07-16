# MySQL 
> MySQL은 세계에서 가장 많이 쓰이는 오픈 소스의 관계형 데이터베이스 관리 시스템이다.  
## MySQL의 구조
### 데이터베이스 서버
- 스키마를 그룹핑한 것 (MySQL을 설치한다는 것은 MySQL 데이터베이스 서버를 설치한 것과 같음)
### 스키마
- 서로 연관된 테이블(데이터)을 그룹핑 하는 것
### 테이블
- 데이터를 저장하는 곳

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
+---------------------------+
```
### table 생성
```
> create table topic (
id int(11) not null auto_increment,
title varchar(100) not null,
description text null,
created datetime not null,
author varchar(15) null,
profile varchar(200) null,
primary key(id)); 
```
#### Primary Key (주요 키)
- 값이 고유하고 중복되지 않음.
- 각각의 행(row)을 식별하는 식별자

#### auto_increment
- 중복되지 않는 값을 자동으로 만들어줌.

### table 확인
table 필드,필드 속성 조회
```
> desc topic;
### 데이터 생성
```
> insert into topic (title,description,created,author ,profile) values ('MySQL','Mysql is...',NOW(),'yejin','i am yejin');
```
### 데이터 조회 
```
# 일반 조회
> select * from topic;

# 조건식
> select * from topic where id = 1;

# 정렬 (내림차순)
> select * from topic order by id desc; 

# 제한
> select * from topic limit 5;
```
+-------------+--------------+------+-----+---------+----------------+
| Field       | Type         | Null | Key | Default | Extra          |
+-------------+--------------+------+-----+---------+----------------+
| id          | int(11)      | NO   | PRI | NULL    | auto_increment |
| title       | varchar(100) | NO   |     | NULL    |                |
| description | text         | YES  |     | NULL    |                |
| created     | datetime     | NO   |     | NULL    |                |
| author      | varchar(15)  | YES  |     | NULL    |                |
| profile     | varchar(200) | YES  |     | NULL    |                |
+-------------+--------------+------+-----+---------+----------------+
```

### 데이터 생성
```
> insert into topic (title,description,created,author ,profile) values ('MySQL','Mysql is...',NOW(),'yejin','i am yejin');
```
### 데이터 조회 
```
# 일반 조회
> select * from topic;

# 조건식
> select * from topic where id = 1;

# 정렬 (내림차순)
> select * from topic order by id desc; 

# 제한
> select * from topic limit 5;
```

### 데이터 수정
```
update topic set description = 'oracle is ..' , title = 'Oracle' where id = 1;
```

### 데이터 삭제
```
delete from topic where id = 1;
```
###  그룹 함수
#### COUNT()
컬럼의 총 항목 수를 구해줌.
```
> SELECT COUNT(*) FROM employees;
+----------+
| COUNT(*) |
+----------+
|   300024 |
+----------+
```
#### MAX()
컬럼 중 가장 큰 값을 찾아줌.
```
> SELECT MAX(emp_no) FROM employees;
+-------------+
| MAX(emp_no) |
+-------------+
|       14999 |
+-------------+
```


