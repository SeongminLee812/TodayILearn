# SQL

데이터베이스 설치

- yum install mariadb-server(마리아 디비 설치)
- systemctl enable mariadb --now
- mysql -u root -p
- 비밀번호 설정

```
 -mysql_secure_installation
...
Enter current password for root (enter for none): "Enter 입력"
...
Set root password? [Y/n] y
...
New password: root
Re-enter new password: root
...
Remove anonymous users? [Y/n] y
...
Disallow root login remotely? [Y/n] y
...
Remove test database and access to it? [Y/n] y
...
Reload privilege tables now? [Y/n] y
...
```

### 데이터 베이스의 구성

-표로 구성되어 있다.

-필드이름

-레코드(=row) : 실제 데이터가 들어가는 곳

-컬럼 : 세로부분 어느곳이든

-필드 값 : 실제 들어가는 데이터 값

-(데이터베이스는 엑셀과 유사하기에 엑셀파일을 데이터 베이스에 넣을 수 있다)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ae95bc4b-eb4b-4e55-b1de-b9e3dbacbe52/Untitled.png)

- 데이터 베이스 사용 목적 : 정보관리, 웹 관련성

### 데이터베이스의 언어

- 데이터베이스를 사용하기 위해서는 데이터베이스 언어를 알아야함.(아래 세가지)

- **데이터 정의 언어(DDL, Date Define Language)**

  - 데이터베이스 or 테이블의 구조를 정의하는 언어
  - 데이터가 모여서 테이블이 되고, 테이블이 모여서 데이터베이스가 된다.
  - 사용구문

  -**CREATE** : 생성(데이터베이스, 테이블, 사용자 생성구문)

  -**ALTER** : 수정

  -**DROP** : 삭제

  ```
  -데이터 베이스 생성명령어
  
  → `CREATE DATABASE *database-name*;`
  ```

  -세미콜론(;)은 구문을 마친다는 의미

  -세미콜론 입력을 하지 않으면 구문이 종료되지 않는다.

  -명령어는 대소문자를 구별하지 않으나, 필드 이름은 대소문자를 구별한다.

  - **테이블 생성하기**

    테이블 생성을 위해 데이터베이스 접근 MariaDB [(none)]> USE TEST_DB; Database changed MariaDB [TEST_DB]>

    - 테이블 생성 -형식

      ```sql
      CREATE TABLE *table-name*(
      			field-name1 data-type option
            field-name2 data-type option
            ...
          );
      ```

      ```sql
      MariaDB [TEST_DB]> CREATE TABLE user (
      -> num int AUTO_INCREMENT,
      -> name varchar(50) not null,
      -> email varchar(50),
      -> primary key(num)
      -> );
      ```

  - `num int AUTO_INCREMENT`

  필드이름 num, 정수형 데이터 int, 값 자동증가

  - `name varchar(50) not null,`

  -필드이름 name, 가변 데이터 varchar(50), 값이 비어있으면 안됨 not null

  - `primary key(*num*)` → 기본키로 num필드를 지정한다는 의미, 동일한 항목이 있을 경우 구분 지을 고유의 값이 필요, 기본키는 중복되는 값이 존재하지 않아야 함.

    사람으로 비유하면 주민등록 번호임

  - `char, varchar` : 문자열 자료형

    *이름이 총 6데이터를 사용한다고 가정하면 44데이터가 남음, 데이터의 남는 공간을 어떻게 처리하느냐에 따라 char, varchar가 달라짐

    - char (고정 사이즈)

      데이터의 남은 공간을 모두 공백으로 채움

      공간의 낭비 발생 가능성

      그러나 빨리 찾을 수 있다.

    - varchar (가변 사이즈)

      ```
      데이터의 남는 공간은 공백이 추가되지 않음.(타이트하게 사용)
      
      실질적인 데이터(6) + 길이 정보(1) 을 같이 저장
      ```

      길이 정보를 먼저 본 후 실질적인 데이터를 보기 때문에 시간이 오래 걸림

      ```
      용량면에서 varchar가 유리, 속도면에서 char가 유리
      ```

  - 테이블 구성보기 : describe *table*;

  - **사용자 생성**

    - 형식

    - ```
      CREATE USER *user*@*host* IDENTIFIED BY ‘*password*’;
      ```

      - 사용자 확인 1 (데이터베이스 내 데이터 확인)
        - 사용자는 TEST_DB에 있는 것이 아닌 다른 한 곳에 저장중임(mysql 데이터베이스)
        - `USE mysql;`
        - `MariaDB [mysql]> select user, host, password from user where user='user01’`
      - 사용자확인 2(실제 로그인)
        - exit으로 나간 후,
        - `mysql -u user01 -p`
        - `Enter password : ‘user01’`
      - localhost는 현지 작업자기 때문에 생략이 가능하다.

  - **테이블 수정구문(ALTER)**

    -ALTER 구문 사용

    - 테이블에 필드 추가

      -형식 : `ALTER TABLE *table-name* ADD f*ield-name* data-type **[option]*;*`

    - 테이블 내 필드 제거 (테이블이나 데이터베이스 자체를 지울때는 drop;을 사용)

      -형식 :  `ALTER TABLE *table-name* DROP COLUMN *field-name*;`

    - 테이블 필드 이름 수정(이름 뿐 아니라 옵션, 데이터 타입 등 다른 속성도 변경가능)

      -형식 : `ALTER TABLE *table-name* CHANGE *old-field-name new-field-name* data-type [option];`

      -만약 올드네임과 뉴네임만 넣고 나머지 생략하면 오류 발생 →  data-type과 [option]을 다 넣어야함.

      수정후에는 항상  describe로 테이블 확인하는 습관을 가지자!!

  - **데이터베이스, 테이블 삭제 구문(DROP)**

    - 형식 : DROP {DATABASE | TABLE} *name*

    ```sql
     **DROP TABLE edu;
    ```

    - show tables로 삭제 되었는 지 확인

- **데이터 조작 언어(DML, Data Manipulation Language)**

  - 데이터를 테이블에 삽입, 조회, 수정, 삭제하는 언어
  - 사용할 수 있는 구문
    - INSERT(삽입)
    - SELECT(조회)
    - UPDATE(수정)
    - DELETE(삭제)

  1. **데이터 삽입**

  - 형식
    - `INSERT INTO *table-name* (*field-name1, field name2, ...)* VALUES (’*value1’, ‘value2’,* ...)`
    - value부분은 문자열이기 때문에 따옴표(작은따옴표, 큰따옴표)로 묶어줘야 한다.

  1. **데이터 조회**

  - 형식
    - `SELECT *field-name1, field-name2*, ... FROM *table-name* [WHERE condition]`
    - WHERE 문은 생략가능 (예를 들어 id 1번부터 10번까지만 조회할 경우),
    - WHERE절
      - WHERE절은 조건절
      - 조건 비교시 사용
      - 조건에 맞는 데이터를 삽입, 조회, 수정, 삭제
      - 비교 연산자
        - a = b → a와 b가 같다
        - a > b → a가 b보다 크다
        - a >= b → a가 b보다 크거나 같다.
        - 예시
    - SELECT * FROM *table-name*; → 모든 필드를 보여줌.

  1. **데이터 수정**

  - 형식
    - UPDATE *table-name* SET *field-name*=*value* WHERE condition
    - update 테이블네임 set 필드네임=수정된내용 where 조건
    - 필드네임에는 필드값이 아닌 필드 네임이 들어가고, 수정된 내용에는 필드값에 들어갈 내용이 들어간다.

  1. 데이터 삭제

  - 형식
    - DELETE FROM *table-name* WHERE condition;
    - 해당 테이블 내에 condition 값을 가진 데이터를 전부 삭제

- **데이터 제어 언어(DCL, Data Control Language)**

  - 사용자에게 데이터베이스에 대한 접근을 제어하는 언어

    - root 사용자 → 최고 권한을 갖는 사용자
    - root 사용자로 직접 사용하기에는 보안 상 위험할 수 있다.(탈취될 경우 보안적 문제 발생)→ root 사용자를 직접 이용하지 말자 → 일반 사용자에게 권한을 부여해준다( 철수 - 데이터 생성권한, 영희 - 데이터 삭제 권한 등 권한을 분산시키거나, 사용자의 직급에 따라 권한을 많거나 적게 줄 수 있음.)

  - 사용할 수 있는 구문(권한 부여와 회수는 root사용자만 가능)

    - GRANT(권한부여)

      - 형식

        ```sql
        GRANT permission ON *database-name.table-name* TO *user-name@host*;
        ```

        - permission : 권한
          - 여러 권한을 부여할 경우 쉼표로 구분하여 가능 (select, insert)
        - 데이터베이스 안의 특정 테이블
        - *table-name*부분에 ‘*’을 지정해주면 모든 테이블에 대한 권한(데이터 베이스 전체에 대한 권한 부여)
        - FLUSH PRIVILEGES; → 권한테이블 새로고침.
        - 권한을 부여 해준 후 권한에 대한 내용들을 FLUSH PRIVILEGES; 명령어를 사용하여 새로고침 시켜줘야한다.
        - 사용자 권한확인 → SHOW GRANTS FOR *user-name@host-name*;
        - 

        

    - REVOKE(권환회수)

      - 형식
        - REVOKE permission ON *database-name.table-name FROM* *user-name@host*;
        - 권한부여와 똑같지만, to 대신 from이 오면 된다.
        - 마찬가지로   FLUSH PRIVILEGES; 입력하여 권한 새로고침

### 데이터베이스 백업 및 복원

- 데이터베이스를 백업하고 복원하는 방법

  - 물리적방법

    - 파일 전체를 복사하고 붙여넣기하는 방법
    - 파일 자체를 백업, 복원
    - 장점 : 속도가 빠름
    - 단점 : 운영체제 호환(우분투에서 센토스로 복원하면 문제 발생 가능성), 데이터베이스 버전, 하드웨어 사양 등에 민감하다.

  - 논리적방법

    - 파일 형태로 만들어서 백업하고 복원하는 방법

    - 데이터베이스를 파일화 시키는 것

    - 복원할 때도 파일 형태의 파일을 사용하여 복원

    - 방법(리눅스 명령줄에서 진행)

      - exit으로 빠져 나와서 명령줄 나오게끔

      ```sql
      [root@localhost ~]# mysqldump -u *user-name* -p *database-name > file-name*
      ```

      - fime-name → 어떤 파일 이름으로 저장할 건지
      - 확인 → ls 명령어 입력하여 *file-name*으로 된 파일이 있는 지 확인(TEST_DB.dump파일로 저장함)
      - 

      ```
        ![Untitled](<https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e7b2d9e4-2409-49d0-a543-4fc150b192a6/Untitled.png>)
      ```

      - 파일 내용 확인 → CAT *file-name*
      - 

      ```
        ![Untitled](<https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcfce55c-11f0-4627-8131-24dda4f969e1/Untitled.png>)
      ```

      - 내가 입력한 명령어 확인할 수 있음

    - 복원(리눅스 명령줄에서 진행)

      - *database-name*은 다른 이름으로 지정할 수 있음

      - *database-name*으로 된 데이터 베이스가 생성되어 있어야 복원 가능

        ```sql
        mysql -u root -p
        create database TEST_DB_restore;
        show databases;
        exit
        ```

      - 위 명령어로 복원할 데이터베이스를 우선 만든 후,

      - 형식

        ```sql
        [root@localhost ~]# mysql -u *user-name* -p *database-name* < *file-name*
        ```

        ```sql
        mysql -u root -p TEST_DB_restore < TEST_DB.dump
        ```

      - 위와 같이 TEST_DB_restore 데이터 베이스에 백업파일(dump파일)을 넣어주면 백업 완성

      - show, describe,, select 명령어로 복원이 잘 되었는 지 확인까지!

      - 백업할 때는 mysqldump -u *user-name* -p *database-name* > *file-name*로 방향이 데이터베이스>새로 생성할 파일으로 가고 복원할 때는 mysql -u *user-name* -p *database-name* < *file-name*로 방향이 데이터베이스 **<** 내용을 복원할 파일로 방향이 바뀜에 유의하자