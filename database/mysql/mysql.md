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
```database
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
```database
> use databasename;
# use mysql;
```

### Database 생성
명령어는 되도록 대문자로 작성함.
```database
> CREATE DATABASE db;
```

### table 조회
```database
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
```database
> create table topic (
id int(11) not null auto_increment,
title varchar(100) not null,
description text null,
created datetime not null,
author varchar(15) null,
profile varchar(200) null,
primary key(id)); 


# 다른 테이블을 복사한 것과 같은 테이블을 생성
> create table citycopy like city;

```
#### Primary Key (주요 키)
- 테이블의 행을 유일하게 특정할 수 있는 유니크한 값을 저장하는 열.
- 값이 고유하고 중복되지 않음.
- 각각의 행(row)을 식별하는 식별자

#### auto_increment
- 중복되지 않는 값(유니크한 값)을 자동으로 만들어줌.

### table 확인
table 필드,필드 속성 조회
```database
> desc topic;
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
```database
 > insert into topic (title,description,created,author ,profile) values ('MySQL','Mysql is...',NOW(),'yejin','i am yejin');

# 다른 테이블의 행을 참조해 데이터를 생성
# 테이블의 구조가 같아야함.
> insert into citycopy select * from city; # city 테이블의 모든 데이터가 citycopy에 생성(복제)됨. 
```
### 데이터 조회 
```database
# 일반 조회
> select * from topic;

# 조건식
> select * from topic where id = 1;

# 정렬 (내림차순)
> select * from topic order by id desc; 

# 제한
> select * from topic limit 5;

# 중복되는 값을 제거
> select distinct District from city where CountryCode = 'KOR';
> select district from city where CountryCode = 'KOR' group by district; # group by를 통해 중복 제거
```
### 데이터 수정
```database
> update topic set description = 'oracle is ..' , title = 'Oracle' where id = 1;
```

### 데이터 삭제
```database
> delete from topic where id = 1;
```

### JOIN
<b><i>관계형 데이터베이스의 꽃</i></b>  
```database
>  select topic.id as topic_id,title,description,created,name,profile from topic left join author on topic.author_id = author.id;
```

### GROUP BY
대상이 되는 데이터를 그룹으로 나눠 집계할 수 있음.
```database
# 행정구역을 그룹지어서 각 행정구역의 도시의 개수를 구함.
>  select district , count(*) from city where CountryCode = 'KOR' group by district;
+---------------+----------+
| district      | count(*) |
+---------------+----------+
| Cheju         |        1 |
| Chollabuk     |        6 |
| Taegu         |        1 |
| Taejon        |        1 |
+---------------+----------+
```

#### Having
그룹함수로 집계한 값을 조건으로 선택하고 싶을 경우 having으로 조건을 지정함.
```database
# 행정구역을 그룹지어 도시의 수가 6개인 행정구역만 반환.
> select district , count(*) from city where CountryCode ='KOR' group by district having count(*) = 6;
+---------------+----------+
| district      | count(*) |
+---------------+----------+
| Chollabuk     |        6 |
| Chungchongnam |        6 |
+---------------+----------+
```

###  그룹 함수
그룹함수는 select,order by,having 절에서만 작성 가능함.

#### COUNT()
컬럼의 총 행의 개수를 구해줌.
```database
> SELECT COUNT(*) FROM employees;
+----------+
| COUNT(*) |
+----------+
|   300024 |
+----------+
```
#### MAX()
칼럼의 총 행 중 가장 큰 값을 찾아줌.
```database
> SELECT MAX(emp_no) FROM employees;
+-------------+
| MAX(emp_no) |
+-------------+
|       14999 |
+-------------+
```

#### MIN()
칼럼의 총 행 중 가장 작은 값을 찾아줌.
```database
> select min(Population) from city where CountryCode = 'KOR';
+-----------------+
| min(Population) |
+-----------------+
|           92239 |
+-----------------+
```

#### AVG()
칼럼의 총 데이터의 평균 값을 구함.
```database
> select avg(Population) from city where CountryCode = 'KOR';
+-----------------+
| avg(Population) |
+-----------------+
|     420554.6957 |
+-----------------+
```

#### GROUP_CONCAT()
반환되는 행을 문자열을 합해 한개의 행으로 반환함.
```database
> SELECT group_concat(Name) from city where CountryCode='KOR' and district = 'chollabuk';
+-------------------------------------------+
| group_concat(Name)                        |
+-------------------------------------------+
| Chonju,Iksan,Kunsan,Chong-up,Kimje,Namwon |
+-------------------------------------------+
```

## refer
[생활코딩 database2](https://www.youtube.com/watch?v=h_XDmyz--0w&list=PLuHgQVnccGMCgrP_9HL3dAcvdt8qOZxjW&index=1)
