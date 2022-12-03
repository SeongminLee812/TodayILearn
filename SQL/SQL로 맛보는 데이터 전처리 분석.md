# SQL 문법

- SELECT 칼럼, 계산 값
- FROM 테이블 명
- WHERE 조건
- GROUP BY 그룹화
- HAVING 그룹화에 사용되는 조건

## SELECT
`SELECT 호출하려는 칼럼 FROM DB명.TABLE명`
`SELECT COUNT(상품번호) FROM DB명.테이블명`
`SELECT 집계함수 FROM DB명.테이블명

- 모든결과 조회 : \*
- 여러칼럼 출력 : , 사용
- 칼럼명을 변경해 조회 : AS
	- `SELECT 칼럼명1 AS 변경 칼럼명 FROM DB.TABLE`
- DISTINCT : 중복 제외
	- `SELECT DISTINCT 컬럼명 FROM DB.TABLE`

## FROM
FROM DB명.TABLE명 을 사용해도 되지만, USE DB명; 으로 DB를 특정해놓고 FROM TABLE명만 사용해도 된다

## WHERE
- 조건문
- 미국에서 판매되는 상품 조회 
	- `SELECT 상품번호 FROM DB.TABLE WHERE 판매국가 = '미국'`
- 1. BETWEEN
	- 특정 칼럽 값이 시작점~끝점인 데이터만 출력할 수 있는 조건을 생성할 수 있다.
	- `SELECT * FROM DB.TABLE WHRE 칼럼 BETWEEN 시작점 AND 끝점`
- 2. 대소관계 표현
| 연산자 | 설명     |
| ------ | -------- |
| =      | 동일하다 |
| >      | 초과     |
| >=     | 이상     |
| <      | 미만     |
| <=     | 이하     |
| <>     | 같지않다 |
- 3. IN
	- 칼럼의 값이 '값1' 또는 '값2'인 데이터 출력
	- `SELECT 칼럼명 FROM 테이블명 WHERE 칼럼명 IN (값1, 값2)`
	- 
 