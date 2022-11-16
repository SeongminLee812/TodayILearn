# 1. 데이터 정의 언어(DDL, Data Define Language)
- 데이터 베이스, 테이블의 구조를 정의하는 언어
- CREATE : 생성
	- 데이터 베이스 생성
		- `CREATE DATABASE *database-name*;`
	- 테이블 생성
```sql
CREATE TABLE table-name( field-name1 data-type option field-name2 data-type option ... );
```
- ALTER : 수정
```sql
ALTER TABLE table-name ADD filed-name data-type [option];
```
	- 필드(컬럼) 삭제 ALTER TABLE table-name DROP COLUMN filed-name;
	- 필드 이름 수정 ALTER TABLE table-name CHANGE old-field-name new-field-name data-type [option];

- DROP : 삭제
	- 데이터 베이스, 테이블 삭제 구문
	- DROP TABLE edu;

# 2. 데이터 조작 언어(DML, Data Manipulation Language)
- INSERT(삽입)
```sql
INSERT INTO table-name (field-name1, field name2, ...) VALUES (’value1’, ‘value2’, ...)
```
	- VALUES 뒷 부분은 문자열 이기 때문에 따옴표로 묶어줌

- SELECT(조회)
```sql
SELECT field-name1, field-name2, ... FROM table-name [WHERE condition]
```

- UPDATE(수정)
```sql
UPDATE table-name SET field-name=value WHERE condition
```

- DELETE(삭제)
```sql
DELETE FROM table-name WHERE condition;
```

# 3. 데이터 제어 언어(DCL, Data Control Language)

- GRANT(권한 부여)
```sql
GRANT permission ON database-name.table-name TO user-name@host;
```
	- 명령어 작성 후 FLUSH PRIVILEGES;를 입력하여 변경사항 반영시켜야 함.
	- SHOW GRANTS FOR user-name@host-name; 으로 권한 확인 가능

- REVOKE(권환 회수)
```sql
REVOKE permission ON database-name.table-name FROM user-name@host;
```
	- GRANT랑 같은 구문이나 TO 대신 FROM이 오면 됨
	- 명령어 작성 후 FLUSH PRIVILEGES;를 입력하여 변경사항 반영시켜야 함.

# 데이터베이스 백업 및 복원
- 물리적 방법
	-   파일 전체를 복사하고 붙여넣기하는 방법
	-   장점 : 속도가 빠름
	-   단점 : 운영체제 호환(우분투에서 센토스로 복원하면 문제 발생 가능성), 데이터베이스 버전, 하드웨어 사양 등에 민감하다.

- 논리적 방법
	-   데이터베이스를 파일화 시켜 백업하고 복원하는 방법
	-   복원할 때도 파일 형태의 파일을 사용하여 복원
- 백업 파일 생성
```terminal
[root@localhost ~]# mysqldump -u user-name -p database-name > file-name
```

- 복원하기
	- 우선 mysql에서 빈 데이터 베이스를 생성해야함
	- 터미널에서 아래 명령어 입력
```terminal
[root@localhost ~]# mysqldump -u user-name -p database-name > file-name

mysql -u root -p TEST_DB_restore < TEST_DB.dump
```
