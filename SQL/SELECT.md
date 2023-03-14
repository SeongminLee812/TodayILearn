-   조회 기능.
-   sql에서 대부분의 구문은 대문자로 사용(가독성을 위해서)
-   쿼리가 끝나는 부분에 ; 을 입력하여 가독성을 올려준다.

`SELECT column_name FROM table_name`

-   컴퓨터는 뒤에서부터 읽음, 테이블 이름을 조회하여, 그 태이블 내에 컬럼을 조회한다.
-   컬럼명은 쉼표로 구분하여 여러개의 컬럼을 조회할 수 있다 → 내가 입력한 순서대로 반환됨

`SELECT * FROM table_1`

-   모든 테이블을 조회
-   일반적으로 정말 모든 내용을 원하는 것이 아니라면 \*을 사용하는 것은 좋지 않다 → 데이터 베이스 서버와 애플리케이션에 트래픽이 발생, 느려질 수 있기 때문
-   특정 열만 필요하다면 가급적 그 열만 조회

`SELECT first_name, last_name, email FROM customer;`

-   custmer 테이블에서 first_name, last_name, email 컬럼 조회

## 고유한 값만 조회하는 DISTICT

-   고유한 값, 중복을 제거한 값만 조회하고 싶을 경우 DISTINCT 사용

`SELECT DISTINCT column FROM table`

-   조회하고 싶은 컬럼 앞에 DISTINCT 를 작성
-   DISTINCT(column)과 같이 괄호를 사용할 수도 있음 → 무엇을 호출하고자 하는 지 명확하게 알기 위함
-   그 테이블, 그 열에 고유한 이름은 몇개인가?

## COUNT

-   특정 쿼리 조건에 맞는 입력 행의 개수를 구하는 함수
-   특정 열의 개수를 구할 수도 있고, COUNT(*)로 모든 열을 구할 수도 있다.
-   `SELECT COUNT(name) FROM table;`
-   어떤 열을 선택하든 같은 결과를 반환한다 → 행의 개수는 열마다 모두 같으니까! → 따라서 보통 COUNT(\*)을 사용/ 그러나 문제를 해결하는 과정에서는 특정 열의 이름을 사용하는 것이 나중에 쿼리를 읽을 때 편할 수도 있다.
-   다른 명령어와 함께 사용하는 것이 훨신 유용하다
-   `SELECT COUNT(DISTINCT name) FROM table;` 와 같이 사용하면 고유한 name이 몇개 있는 지 개수를 셀 수 있다.

## WHERE

-   열에 조건을 지정하여 그에 해당하는 행을 반환
-   `SELECT column1, column2 FROM table WHERE conditions;`
-   FROM 절 뒤에 WHERE 절이 나옴
-   조건을 적용하여 필터링 할 때는 2개 이상의 열을 조회해야 효과가 있다.

### 비교 연산자

-   ex) ‘3,000원이 넘는 제품은 뭐가 있나?’

| Operator | Description |
| -------- | ----------- |
| =        | 같다        |
| <       | 작다        |
| >        | 크다        |
| <=       | 작거나 같다 |
| >=       | 크거나 같다 |
| <> or !  | 같지 않다            |

### 논리 연산자

-   AND : 둘 다 참
-   OR : 둘 중 하나만 참
-   NOT : 지정한 조건과 반대 조건

```sql
SELECT * FROM film
WHERE rental_rate > 4 AND replacement_cost >= 19.99
```

-   rental_rate이 4보다 크고, replacement_cost가 19.99보다 크거나 같은 모든 열 조회