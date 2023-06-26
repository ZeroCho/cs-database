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
- 데이터를 저장하는 2차원 공간
- 엑셀이라고 생각해보자(row, column)

### 정규화(Normalization)
- 테이블을 정규형으로 만드는 걸 정규화라고 한다.
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

- 나중에 JOIN으로 합침

### ERD
관계 생각하기
1. 1:N(일대다)
2. 1:1(일대일)
3. M:N(다대다)

[실전링크](https://www.erdcloud.com/d/QqKNELMS6e3AB3xia)

#### Key
- PK(기본키, primary key)
- FK(외래키, foreign key)
- 복합키(Composite Key, 두 개 이상의 키가 기본키가 되는 것)
  
동명이인이 생겨버리면...?

|아이디|이름|전화번호|
|---|---|----|
|1|제로초|010-1234-5678|
|1|제로초|010-2345-6789|
|2|제로초|010-3456-7890|

아이디는 중복되면 안 됨

|아이디(PK)|이름|
|---|---|
|1|제로초|
|2|제로초|

|아이디(PK)|사원아이디(FK)|전화번호|
|---|---|----|
|1|1|010-1234-5678|
|2|1|010-2345-6789|
|3|2|010-3456-7890|

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

## CREATE TABLE
DDL(Definition), DQL(Query), DML(Manipulation), DCL(Control), TCL(Transaction)
  - [참고](https://www.geeksforgeeks.org/sql-ddl-dql-dml-dcl-tcl-commands/)

fk 없는 거 먼저 만들기
```
CREATE TABLE `zerocho`.`role` (
  `id` INT(11) UNSIGNED NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(10) NOT NULL,
  `min_salary` INT UNSIGNED NOT NULL DEFAULT 2500,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `name_UNIQUE` (`name` ASC) VISIBLE,
  UNIQUE INDEX `id_UNIQUE` (`id` ASC) VISIBLE)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4;
```

ON UPDATE, ON DELETE 옵션들 기억하기
```
CREATE TABLE `employee` (
  `id` int unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(45) NOT NULL,
  `email` varchar(100) NOT NULL,
  `salary` int unsigned NOT NULL,
  `team` varchar(20) NOT NULL,
  `quit_date` date DEFAULT NULL,
  `created_at` datetime DEFAULT CURRENT_TIMESTAMP,
  `role_id` int unsigned DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `email_UNIQUE` (`email`),
  KEY `employee_role_fk_idx` (`role_id`),
  CONSTRAINT `employee_role_fk` FOREIGN KEY (`role_id`) REFERENCES `role` (`id`) ON DELETE SET NULL ON UPDATE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci COMMENT='사원테이블'
```


### ALTER, DROP, TRUNCATE

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
나중에 조회 시에는
```
SELECT * FROM employee WHERE quit_date IS NULL
```

## ERDCloud

가져오기 메뉴에서 다음 SQL 복붙!
```
CREATE TABLE `role` (
	`id`	int	NOT NULL	COMMENT '직책아이디',
	`name`	varchar(10)	NOT NULL	COMMENT '직책이름',
	`min_salary`	int	NOT NULL	DEFAULT '2500'	COMMENT '최저연봉'
);

CREATE TABLE `employee` (
	`id`	int	NOT NULL,
	`name`	varchar(45)	NOT NULL,
	`email`	varchar(100)	NOT NULL,
	`salary`	int	NOT NULL,
	`team`	varchar(20)	NOT NULL,
	`quit_date`	date	NULL,
	`created_at`	datetime	NULL,
	`role_id`	int	NOT NULL	COMMENT '직책아이디'
);

CREATE TABLE `employee_info` (
	`id`	int	NOT NULL,
	`address`	varchar(100)	NULL,
	`etc`	text	NULL
);

ALTER TABLE `role` ADD CONSTRAINT `PK_ROLE` PRIMARY KEY (
	`id`
);

ALTER TABLE `employee` ADD CONSTRAINT `PK_EMPLOYEE` PRIMARY KEY (
	`id`
);

ALTER TABLE `employee_info` ADD CONSTRAINT `PK_EMPLOYEE_INFO` PRIMARY KEY (
	`id`
);

ALTER TABLE `employee_info` ADD CONSTRAINT `FK_employee_TO_employee_info_1` FOREIGN KEY (
	`id`
)
REFERENCES `employee` (
	`id`
);
```

## SELECT
```
SELECT * FROM employee
```

### where
```
SELECT * FROM employee WHERE name = ?
```
```
SELECT * FROM employee WHERE created_at BETWEEN '2023-05-01' AND '2023-06-30'
```
```
SELECT * FROM employee WHERE salary > 3000 OR team = '기획팀'
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
```
SELECT id as no, name FROM employee WHERE name = ? ORDER BY salary, role_id
```
### limit
```
SELECT id as no, name FROM employee WHERE name = ? ORDER BY salary LIMIT 10
```
### offset
```
SELECT id as no, name FROM employee WHERE name = ? ORDER BY salary DESC LIMIT 10 OFFSET 10
```
커서 방식 vs OFFSET 방식
```
SELECT id as no, name FROM employee WHERE name = ? AND id < 8 ORDER BY salary DESC LIMIT 10
```

### group by, having
```
SELECT AVG(salary) FROM employee WHERE name = ? GROUP BY team HAVING team = '개발팀' OR team = '디자인팀'
```
### count, avg, min, max, sum

## JOIN
### INNER JOIN
null 나올 수 없음
```
SELECT ... JOIN ... ON ...
```
### LEFT/RIGHT JOIN
null 나올 수 있음
```
SELECT ... LEFT JOIN ... ON ...
```

### FULL OUTER JOIN
```
SELECT ... LEFT JOIN ... ON ...
UNION
SELECT ... RIGHT JOIN ... ON ...
```
### CROSS JOIN
```
SELECT ... FROM a JOIN b
SELECT ... FROM a CROSS JOIN b
SELECT ... FROM a, b
```

# INDEX
- (a, b, c) index
- a를 안 쓰면 무용지물
```
CREATE INDEX
DROP INDEX
```
# TRANSACTION
```
START TRANSACTION;

COMMIT;
ROLLBACK;
```

## ACID
- Atomicity
- Consistency
- Isolation
- Durability

## LOCK

# Stored Procedure
```
DELIMITER //
CREATE PROCEDURE increaseMinSalary(money INT, rid INT)
BEGIN
	START TRANSACTION;
    UPDATE zerocho.role SET min_salary = money WHERE id = rid;
    UPDATE zerocho.employee SET salary = money WHERE role_id = rid AND salary < money;
	COMMIT;
END //
DELIMITER ;

EXEC procedure_name;
```

# Trigger
```
DELIMITER //
CREATE TRIGGER updateSalaryWhenMinSalaryChange AFTER UPDATE ON zerocho.role
FOR EACH ROW
BEGIN
	UPDATE zerocho.employee SET salary = NEW.min_salary WHERE role_id = NEW.id AND salary < NEW.min_salary;
END //
DELIMITER ;
```
```
SHOW TRIGGERS;
DROP TRIGGER updateSalaryWhenMinSalaryChange
```

# View
```
CREATE OR REPLACE VIEW zerocho.ep_view
AS SELECT employee_id, project_id, role_id, e.name as employee_name, salary, team 
FROM zerocho.employee e LEFT JOIN zerocho.role r ON e.role_id = r.id
LEFT JOIN zerocho.employee_project ep ON e.id = ep.employee_id;
```

# ETC
## NoSQL
MongoDB, Redis
## replication
## sharding
## CAP theorem
