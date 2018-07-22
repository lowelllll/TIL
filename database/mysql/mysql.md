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

### View
> 데이터를 가지지 않는 가상의 테이블

SQL 시점에서는 테이블과 동일하지만 테이블과 같은 데이터는 가지고 있지 않으며, 테이블에 대한 SELECT를 가짐.

#### 뷰 생성
`create view 뷰 이름 (열1,[열2..]) as select 문`

#### 뷰를 사용하는 이점
1. 복잡한 select문을 일일이 매번 기술할 필요가 없음
2. 필요한 열고 ㅏ행만 사용자에게 보여줄 수 있고 갱신시에도 뷰 정의에 따른 갱신으로 한정할 수 있음.
3. 1,2 이점을 데이터 저장없이 (기억장치의 용량을 사용하지 않음) 실현할 수 있음.
4. 뷰를 제거해도 참조하는 테이블은 영향을 받지 않음.

```
# 기존 테이블의 SELECT
> select * from city where countrycode = 'KOR' 
+------+------------+---------------+------------+
| id   | name       | district      | population |
+------+------------+---------------+------------+
| 2332 | Pusan      | Pusan         |    3804522 |
| 2333 | Inchon     | Inchon        |    2559424 |
| 2400 | Mun-gyong  | Kyongsangbuk  |      92239 |
+------+------------+---------------+------------+

# 뷰 생성
> create view citykorea as select * from city where countrycode ='KOR';

# 뷰로 SELECT
> select * from citykorea;
+------+------------+---------------+------------+
| id   | name       | district      | population |
+------+------------+---------------+------------+
| 2332 | Pusan      | Pusan         |    3804522 |
| 2333 | Inchon     | Inchon        |    2559424 |
| 2400 | Mun-gyong  | Kyongsangbuk  |      92239 |
+------+------------+---------------+------------+
```

### 서브쿼리
> 스칼라 값을 데이터처럼 다루거나 수치처럼 취급해 조건문에 이용할 수 있는 것.

```
# 도시의 인구가 평균 이상인 도시의 개수를 구함.
> select count(*) from cirtkorea where population > (select avg(population) from cirtkorea);

# 각 행정구역에서 인구 평균을 구해 각 행정구역 내에서 인구가 평균보다 많은 도시를 구함.
> select district , name, population from cirtkorea as c1 where population > (select avg(population) from cirtkorea as c2 where c1.district = c2.district group by district);
```
솔직히 2번째 이해안감.. help me.. 

#### 스칼라 값
> 하나의 열과 하나의 행으로 구성된 테이블. 즉 <b>단일값</b>으로 구성된 경우

```
# 스칼라 값 예
> select count(*) from city;
+----------+
| count(*) |
+----------+
|     4078 |
+----------+
```

### JOIN
<b><i>관계형 데이터베이스의 꽃</i></b>  

하나의 테이블에 있는 열만으로는 데이터가 충족되지 않는 경우 열을 가지고오는 조작.

```database
>  select topic.id as topic_id,title,description,created,name,profile from topic left join author on topic.author_id = author.id;
```

#### INNER JOIN
> 내부결합, ON으로 지정한 결합조건이 일치하는 행만을 2개의 테이블로부터 가져옴.
```
> select countrylanguage.* , country.name from countrylanguage inner join country on countrylanguage.countrycode = country.code where language = 'Korean';
```

#### OUTER JOIN
> 외부결합, 한쪽 테이블을 기준으로 전체 행을 표시하고 다른 테이블은 일치하면 값이 표시되고 일치하지 않으면 NULL로 표현됨.
```
# LEFT JOIN은 왼쪽 테이블 기준
> select countrylanguage.*, country.name from countrylanguage left outer join country on countrylanguage.countrycode = country.code where language = 'Esperanto';
```
## refer
[생활코딩 database2](https://www.youtube.com/watch?v=h_XDmyz--0w&list=PLuHgQVnccGMCgrP_9HL3dAcvdt8qOZxjW&index=1)
