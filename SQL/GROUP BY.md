

-   데이터가 카테고리별로 어떻게 분포되었는 지 집계하기 위해 필요한 함수

# 집계함수 (Aggregation Function)

-   기본 개념 : 여러 조건을 입력하여 하나의 결과를 반환하는 것
-   집계함수는 SELECT 절이나 HAVING 절에서만 호출됨

1.  AVG() : 평균값 반환
    -   소수점이 굉장히 긴 부동 소수 반환, ROUND()함수를 이용해서 소숫점 설정 가능함

```
SELECT ROUND(AVG(replacement_cost), 3)
FROM film;
/* ROUND : 2개의 파라미터(반올림하고자 하는 값, 반올림후 표시할 자리수) */
```

1.  COUNT() : 값의 수 반환
2.  MAX() : 최대값 반환
    -   괄호 안에 컬럼명을 입력 → 이 값은 하나로만 반환 되기 때문에 SELECT 절에 다른 컬럼을 호출할 수는 없다.
    -   다수의 값을 집계하여 하나만 반환
3.  MIN() : 최소값 반환
	
4.  SUM() : 모든 값의 합계

```sql
SELECT MIN(replacement_cost) FROM film;
```

# GROUP BY

-   카테고리 열을 묶는 함수
-   GROUP BY를 적용할 카테고리 열을 선택 → 연속적이지 않은 값을 가진 컬럼
-   숫자로 되어있거나, 연속적으로 보여도 카테고리 컬럼인 경우가 있음 → 대여료가 0.99달러, 1.99달러, 2.99달러 등으로 나눠져 있는 경우

```sql
SELECT category_col, AGG(data_col)
FROM table
GROUP BY category_col
```

-   GROUP BY 절은 FROM절이나 WHERE절 바로 뒤에 와야함.
-   필요 하다면, GROUP BY 전에 WHERE문을 실행하여 필터링할 수도 있음 (아래 구문은 카테고리 열에 값이 A인것을 제외하고 GROUP BY를 실행)

```sql
SELECT category_col, AGG(data_col)
FROM table
WHERE category_col !='A'
GROUP BY category_col
```

-   SELECT 절에 특정 컬럼을 사용하고 싶다면 GROUP BY 절에 해당 컬럼을 호출해야함. 집계함수를 사용하는 경우만 예외

```sql
SELECT company, division, SUM(sales)
FROM finance_table
GROUP BY company, division
```

-   WHERE 문에 집계함수를 사용할 수 없음 → HAVING을 사용

```sql
SELECT company, division, SUM(sales)
FROM finance_table
WHERE division IN ('marketing', 'transport')
GROUP BY company, division
```

-   ORDER BY

```sql
SELECT company, SUM(sales)
FROM finance_table
GROUP BY company
ORDER BY SUM(sales)
/* 어떤 컬럼 기준으로 ORDER BY를 하더라도, SELECT 절에 호출되어야만 함 */
```

~별 이라는 키워드로 GROUP BY를 이해하기 쉬움

-   아래는 고객 별, 총계

```sql
SELECT customer_id,SUM(amount) FROM payment
GROUP BY customer_id
ORDER BY SUM(amount) DESC;
```

-   날짜별로 조회하기 위해서는, DATE함수를 이용해 타임스탬프를 DATE 형식으로 바꿔줘야함 → 날짜정보만 호출

```sql
SELECT DATE(payment_date),SUM(amount) FROM payment
GROUP BY DATE(payment_date)
ORDER BY SUM(amount)
```

# HAVING

-   집계가 이미 수행된 이후에 자료를 필터링하기때문에 GROUP BY 절 이후에 등장함
-   WHERE문은 GROUP BY 이전에 수행되기 때문에 집계함수를 사용해서 필터링할 수 없음