# Database(DB)
1. 데이터베이스를 왜 배워야 하는가? 
  - 회사에서 가장 중요하게 생각하는게 뭘까?
  - 유튜브 소스코드를 얻었다 쳐보자
2. SQL(Structured Query Language)
  - ANSI SQL
3. DBMS(Database Management System)
  - MySQL 설치하기(PostgreSQL, MSSQL, Oracle, MariaDB, SQLite 등등)
  - [다운로드 링크](https://dev.mysql.com/downloads/installer/)
  - 5버전 8버전 다 괜찮음

## 테이블 만들기

엑셀이라고 생각해보자

### ERD
관계 생각하기
1. 1:N(일대다)
2. 1:1(일대일)
3. M:N(다대다)

[실전링크](https://www.erdcloud.com/d/QqKNELMS6e3AB3xia)

### 정규화(Normalization)
- 엑셀이라고 생각하면 안된다
- 삽입, 수정, 삭제 시 문제가 없나 생각해보기

#### 1NF

한 컬럼에 값이 두 개...?

|이름|전화번호|
|---|----|
|제로초|010-1234-5678<br />010-2345-6789|

<hr>
가로로 배치..?

|이름|전화번호1|전화번호2|
|---|----|---|
|제로초|010-1234-5678|010-2345-6789|

<hr>
|이름|전화번호|
|---|----|
|제로초|010-1234-5678|
|제로초|010-2345-6789|


- JOIN

#### Key
- PK(기본키, primary key)

동명이인이 생겨버리면...?

|아이디|이름|전화번호|
|---|---|----|
|1|제로초|010-1234-5678|
|1|제로초|010-2345-6789|
|2|제로초|010-3456-7890|

- FK(외래키, foreign key)
|아이디|이름|
|---|---|
|1|제로초|
|2|제로초|

|아이디|전화번호|
|---|----|
|1|010-1234-5678|
|1|010-2345-6789|
|2|010-3456-7890|

#### 2NF
- 복합키 모두에 종속되는지 체크

|이름|언어|전화번호|
|---|---|----|
|제로초|JS|010-1234-5678|
|제로초|TS|010-1234-5678|
|제로초|JAVA|010-1234-5678|

#### 3NF
- Key가 아닌 컬럼들의 종속관계(A -> B, B -> C이고 A -> C인 관계가 있다면 A -> B, B -> C 두 개로 분리)

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

#### 역정규화(Denormalization)
- JOIN, JOIN, JOIN
- 삽입, 수정, 삭제가 일어나지 않는 경우
- 서비스 따라 판단

## CREATE TABLE, ALTER TABLE
DDL(Definition), DQL(Query), DML(Manipulation), DCL(Control), TCL(Transaction)
  - [참고](https://www.geeksforgeeks.org/sql-ddl-dql-dml-dcl-tcl-commands/)

```
CREATE TABLE employee (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(20),
  email VARCHAR(50) NOT NULL UNIQUE,
  salary INT NOT NULL DEFAULT 2500,
  team VARCHAR(10) NOT NULL,
  role_id INT NOT NULL REFERENCES role (id),
  quit_at DATE NULL
) ENGINE=InnoDB;
```
```
CREATE TABLE role (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name CHAR(2) NOT NULL UNIQUE,
  min_salary INT NOT NULL,
) ENGINE=InnoDB;
```

## SELECT
```
SELECT * FROM employee
```

### where
```
SELECT * FROM employee WHERE name = ?
```
### projection
```
SELECT id, name FROM employee WHERE name = ?
```
- AS
```
SELECT id as no, name FROM employee WHERE name = ?
```
### order
```
SELECT id as no, name FROM employee WHERE name = ? ORDER BY salary
```
### limit
```
SELECT id as no, name FROM employee WHERE name = ? ORDER BY salary LIMIT 10
```
### group
```
SELECT AVG(salary) FROM employee WHERE name = ? GROUP BY team
```
### count, avg, min, max, sum
## UPDATE
```
UPDATE employee SET salary = ? WHERE name = ?
```
## INSERT INTO
```
INSERT INTO employee (name, salary, team) VALUES ("원초", 5000, "개발"), ("투초", 4000, "디자인")
```
## DELETE
- hard delete
```
DELETE FROM employee WHERE id = ?
```
- soft delete
```
UPDATE employee SET quit_date = NOW() WHERE id = ?
```
```
SELECt * FROM employee WHERE quit_date IS NULL
```
## JOIN
### INNER JOIN
```
```
### LEFT/RIGHT JOIN
```
```

### FULL OUTER JOIN
```
```
# INDEX
(a, b, c) index

```
CREATE INDEX
```
# TRANSACTION
ATM

```
START TRANSACTION;
COMMIT;
ROLLBACK;
```

## ACID

# Stored Procedure
```
CREATE PROCEDURE procedure_name
AS
sql_statement
GO;

EXEC procedure_name;
```

# ETC
## NoSQL
MongoDB, Redis
## replication
## LOCK
## sharding
## CAP theorem
