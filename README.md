# SQL
1. 데이터베이스를 왜 배워야 하는가? 
  - 회사에서 가장 중요하게 생각하는게 뭘까?
  - 유튜브 소스코드를 얻었다 쳐보자
2. MySQL 설치하기(PostgreSQL, MSSQL, Oracle, MariaDB, SQLite 등등)
  - [링크](https://dev.mysql.com/downloads/installer/)
## 테이블 만들기
1. 엑셀이라고 생각해보자
2. DDL(Definition), DQL(Query), DML(Manipulation), DCL(Control), TCL(Transaction)
  - [참고](https://www.geeksforgeeks.org/sql-ddl-dql-dml-dcl-tcl-commands/)
## ERD
관계 생각하기
1. 1:N
2. 1:1
3. M:N
## 정규화(Normalization)
- 엑셀이라고 생각하면 안된다
- 삽입, 수정, 삭제 시 문제가 없나 생각해보기

### 1NF

|이름|전화번호|
|---|----|
|제로초|010-1234-5678<br />010-2345-6789|

<hr>

|이름|전화번호1|전화번호2|
|---|----|---|
|제로초|010-1234-5678|010-2345-6789|

<hr>

|이름|
|---|
|제로초|
|제로초|

|이름|전화번호|
|---|----|
|제로초|010-1234-5678|
|제로초|010-2345-6789|
|제로초|010-3456-7890|

- JOIN

#### Key
- PK
- FK

### 2NF
- 복합키 모두에 종속?

|이름|언어|전화번호|
|---|---|----|
|제로초|JS|010-1234-5678|
|제로초|TS|010-1234-5678|
|제로초|JAVA|010-1234-5678|

### 3NF
- Key가 아닌 컬럼들의 종속관계

|아이디|이름|소속|서비스|
|---|---|---|----|
|1|제로초|네|네이버웹툰|
|2|원초|카|카카오톡|
|3|투초|배|배민1|

- 왜 소속이 아니라 서비스가 키?

<hr>

|이름|소속|서비스|
|---|---|---|
|제로초|네|네이버웹툰|
|원초|카|카카오톡|
|투초|배|배민1|

### 역정규화(Denormalization)
- JOIN, JOIN, JOIN
- 삽입, 수정, 삭제가 일어나지 않는 경우
- 서비스 따라 판단

## SELECT
### where
### projection
### order
### limit
### group
### count
## UPDATE
## INSERT INTO
## DELETE
## JOIN
### INNER JOIN
### LEFT/RIGHT JOIN
### FULL OUTER JOIN
# INDEX
(a, b, c) index
# TRANSACTION
ATM
## ACID
# Procedure
# ETC
## NoSQL
MongoDB, Redis
## replication
## LOCK
## sharding
## CAP theorem
