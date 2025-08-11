# SQL_ADVANCED 0주차 정규 과제 

## Week 0 : 서브쿼리 & CTE

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스 SQL 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_0th_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**(수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 

## SQL_ADVANCED_0th 

### 15.2.15. SubQueries

#### 특히 15.2.15.1 ~ 15.2.15.7 (Scalar, EXISTS, Correlated, Derived 등) 



### 15.2.20 WITH (Common Table Expressions)

- `WITH RECURSIVE`에 대한 내용은 4주차에 공부합니다. 해당 링크에서 `WITH`에 해당하는 부분만 정리해보세요. 




### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**




> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - SubQueries : MySQL 공식문서 
>
> https://dev.mysql.com/doc/refman/8.0/en/subqueries.html
>
> (한국어 버전)
>
> - CTE(공통 테이블 표현식) : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/with.html
>
> (한국어 버전)





## 🏁 주차별 학습 (Study Schedule)

| 주차  | 공부 범위               | 완료 여부 |
| ----- | ----------------------- | --------- |
| 0주차 | 서브쿼리 & CTE          | ✅         |
| 1주차 | 집합 연산자 & 그룹 함수 | 🍽️         |
| 2주차 | 윈도우 함수             | 🍽️         |
| 3주차 | Top N 쿼리              | 🍽️         |
| 4주차 | 계층형 질의와 셀프 조인 | 🍽️         |
| 5주차 | PIVOT / UNPIVOT         | 🍽️         |
| 6주차 | 정규 표현식             | 🍽️         |

<br>



## 프로그래머스 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/131123

> 즐겨찾기가 가장 많은 식당 정보 출력하기 (GROUP BY, SubQuery) : Lev 3

<img width="1438" height="475" alt="image" src="https://github.com/user-attachments/assets/9dba6f2d-909e-449b-ad97-0da448b01389" />


https://school.programmers.co.kr/learn/courses/30/lessons/131115

> 가격이 제일 비싼 식품의 정보 출력하기 (SUM, MAX, MIN, SubQuery) : Lev 2
<img width="1437" height="440" alt="image" src="https://github.com/user-attachments/assets/273aa023-5c46-4101-9258-f8c4a6e60935" />



**두 문제 중에서 한 문제는 SubQuery와 CTE를 사용한 방법을 각각 활용해서 2개의 답변을 제시해주세요**



<!-- 여기까진 그대로 둬 주세요-->

---

 # 1. 서브쿼리

~~~
✅ 학습 목표 :
* SubQueries에 대한 문법을 이해하고 활용할 수 있다.  
~~~

<!-- 새롭게 배운 내용을 자유롭게 정리해주세요.-->
### 15.2.15.1 The Subquery as Scalar Operand
* 스칼라 하위 쿼리란?
 * 한 개의 값만 반환하는 서브쿼리입니다.
 * 이 값을 마치 일반 값(숫자, 문자열 등)처럼 사용합니다.
 * 하지만 NULL 허용 여부는 원래 열의 조건과 무관하게 항상 허용
 * 왜냐하면, 하위 쿼리 결과가 아예 없을 수도 있고, 그 경우는 자동으로 NULL 반환되기 때문

| 항목      | 서브쿼리                                         | **스칼라 서브쿼리**                                            |
| ------- | -------------------------------------------- | ------------------------------------------------------- |
| 결과 개수   | 여러 행 or 열 가능                                 | **1행 1열(값 1개)**                                         |
| 사용 위치   | `IN`, `EXISTS`, `FROM`, `WHERE`, `JOIN` 등 다양 | `=`, `>`, `LIKE` 등 **단일 값 비교 또는 표현식 안**                 |
| 예시      | `(SELECT s1 FROM t2)`                        | `(SELECT MAX(s1) FROM t2)`                              |
| 에러 발생 시 | 결과가 없으면 EMPTY                                | 결과가 **2개 이상이면 에러** (`Subquery returns more than 1 row`) |


~~~sql
CREATE TABLE t1 (s1 INT, s2 CHAR(5) NOT NULL);
INSERT INTO t1 VALUES(100, 'abcde');
SELECT (SELECT s2 FROM t1);
~~~

* 스칼라 하위 쿼리를 사용할 수 없는 상황
 * LIMIT은 정수 리터럴만 인자
 * LOAD DATA 구문도 파일 이름에 리터럴 문자열만 허용

~~~sql
CREATE TABLE t1 (s1 INT);
INSERT INTO t1 VALUES (1);

CREATE TABLE t2 (s1 INT);
INSERT INTO t2 VALUES (2);
~~~
~~~sql
SELECT (SELECT s1 FROM t2) FROM t1;
## 동일: SELECT (TABLE t2) FROM t1; 
~~~
* 바깥쪽 쿼리: FROM t1 → t1 테이블의 행 수만큼 실행됨
* 안쪽 쿼리: (SELECT s1 FROM t2) → 스칼라 하위 쿼리로, 단 한 개의 값을 반환함
* 이때 (TABLE t2)는 t2 테이블에서 첫 번째 컬럼의 첫 번째 값을 하나만 반환
* 하위 쿼리가 함수의 인자로 쓰이는 경우에도 반드시 괄호로 감싸야 함

~~~sql
SELECT UPPER((SELECT s1 FROM t1)) FROM t2;
~~~
* UPPER(1) → 숫자 1 → 문자열 '1'로 처리됨 → 그대로 '1'

### 15.2.15.2 Comparisons Using Subqueries
* 기본형태
~~~sql
비하위쿼리_피연산자 비교연산자 (하위 쿼리)
비교연산자: =  >  <  >=  <=  <>  !=  <=>
혹은 MySQL ver: 비하위쿼리_피연산자 LIKE (하위 쿼리)
~~~
* 서브쿼리는 단순 반복이 아니라, 조인으로는 못 하는 복잡한 비교·조건·집계에 강력
<br>

* 조인으로 표현할 수 없는 대표적인 예시1: MAX()와 비교하는 경우
~~~sql
SELECT * FROM t1
  WHERE column1 = (SELECT MAX(column2) FROM t2);
~~~

* 우리가 원하는 건:t2.column2 전체의 MAX값 하나만 필요함
* 그런데 조인하면: t2의 모든 행이 붙어버림
* 그러면 MAX(t2.column2)는 계속 반복계산되거나, 혹은 그룹핑이 필요해짐
<br>

* 조인으로 표현할 수 없는 대표적인 예시2: 자기 자신을 세는 경우 (자기참조 하위 쿼리)
~~~sql
SELECT * FROM t1 AS t
  WHERE 2 = (SELECT COUNT(*) FROM t1 WHERE t1.id = t.id);

SELECT * FROM t1 As t
  WHERE 2 = (SELECT COUNT(*) FROM t1 WHERE t1.id = t.id)
~~~
* 결론
| 방식        | 작동 방식                                                                | 특징                                 |
| --------- | -------------------------------------------------------------------- | ---------------------------------- |
| **하위 쿼리** | 바깥 쿼리의 **각 행에 대해** 서브쿼리 한 번 실행 → 그 행과 관련된 것만 카운트                     | **필요한 계산만 정확히 1회**                 |
| **조인**    | 테이블 전체를 **자기 자신과 연결해서** 모든 경우의 수(조합)를 만든 후, 그걸 `GROUP BY`로 다시 모아 카운트 | **모든 조합을 만든 뒤 정리**, 과한 연산이 생길 수 있음 |

### 15.2.15.3 Subqueries with ANY, IN, or SOME
* 문법
~~~sql
피연산자 비교연산자 ANY (하위 쿼리)
피연산자 IN (하위 쿼리)
피연산자 비교연산자 SOME (하위 쿼리)
~~~
* `ANY` 
 * 피연산자 비교연산자 ANY (하위 쿼리)
 * 하위 쿼리가 반환하는 값들 중 하나라도 비교 조건을 만족하면 TRUE
~~~sql
SELECT s1 FROM t1 WHERE s1 > ANY (SELECT s1 FROM t2);
~~~
* 만약, t2가 (NULL, NULL, NULL)만 있으면 → 결과는 UNKNOWN(NULL)

* `IN`은 = ANY의 별칭(alias)
~~~sql
SELECT s1 FROM t1 WHERE s1 = ANY (SELECT s1 FROM t2);
SELECT s1 FROM t1 WHERE s1 IN (SELECT s1 FROM t2);
~~~
* IN은 여러 값의 나열도 허용하지만 (= ANY는 안 됨) 예: s1 IN (1, 2, 3)은 가능하지만 s1 = ANY (1, 2, 3)은 불가능
* NOT IN은 ≠ <> ANY가 아님
 * NOT IN은 **<> ANY가 아니라 <> ALL**의 의미와 같음  
<br>
* `SOME`은 ANY의 별칭(alias)
~~~sql
SELECT s1 FROM t1 WHERE s1 <> ANY  (SELECT s1 FROM t2);
SELECT s1 FROM t1 WHERE s1 <> SOME (SELECT s1 FROM t2);
~~~
* any와 some의 영어 표현차이로 인해 <>SOME을 더 선호
<br>

* MySQL 8.0.19 이후: TABLE 구문도 사용 가능
~~~sql
(SELECT s1 from t2) == (TABLE t2)

SELECT s1 FROM t1 WHERE s1 > ANY  (TABLE t2);
SELECT s1 FROM t1 WHERE s1 = ANY  (TABLE t2);
SELECT s1 FROM t1 WHERE s1 IN     (TABLE t2);
SELECT s1 FROM t1 WHERE s1 <> ANY  (TABLE t2);
SELECT s1 FROM t1 WHERE s1 <> SOME (TABLE t2);
~~~

### 15.2.15.4 Subqueries with ALL
* `ALL`
 * 하위 쿼리가 반환하는 모든 값에 대해 비교 연산이 참이면 TRUE를 반환
 * NULL이 끼어 있으면 전체 결과는 UNKNOWN(NULL)
~~~sql
피연산자 비교연산자 ALL (하위 쿼리)
SELECT s1
FROM t1
WHERE s1 > ALL (SELECT s1 FROM t2);
~~~

* t2가 비어있는 경우
1. 

~~~sql
SELECT * FROM t1 WHERE 1 > ALL (SELECT s1 FROM t2);
~~~
 * t2가 비어 있으면 → ALL()은 조건을 무조건 참으로 간주
 * → 결과는 TRUE

2. 
~~~sql
SELECT * FROM t1 WHERE 1 > (SELECT s1 FROM t2);
~~~
* 이건 = ALL()이 아니라 스칼라 서브쿼리
* t2가 비어 있으면 → 하위 쿼리 결과가 없음 → 결과는 NULL

3.
~~~sql
SELECT * FROM t1 WHERE 1 > ALL (SELECT MAX(s1) FROM t2);
~~~
* t2가 비어 있으면 → MAX(s1) 결과가 NULL
* → 그 결과를 ALL에 넣으면 → 결과는 NULL
* **NULL과 빈값의 출력값 차이는 크다** 
<br>
* NOT IN은 <> ALL의 별칭(alias)
~~~sql
SELECT s1 FROM t1 WHERE s1 <> ALL (SELECT s1 FROM t2);
SELECT s1 FROM t1 WHERE s1 NOT IN    (SELECT s1 FROM t2);
~~~

### 15.2.15.5 Row Subqueries
* 기본 개념
 * 스칼라 서브쿼리: 값 하나만 반환 (1행 1열)
 * 컬럼 서브쿼리: 하나의 컬럼을 여러 행 반환 (1열 n행)
 * 행 서브쿼리(Row Subquery): 한 행 전체를 반환, 즉 2개 이상의 열을 1행으로 반환

| 형태                                 | 의미                        | 하위 쿼리 결과 개수    | 에러 발생 가능성    |
| ---------------------------------- | ------------------------- | -------------- | ------------ |
| `(a, b) = (SELECT x, y FROM ...)`  | **행 서브쿼리 (row subquery)** | **반드시 1행 1튜플** | ✅ 2행 이상이면 에러 |
| `(a, b) IN (SELECT x, y FROM ...)` | **다중 행 비교 (튜플 포함 여부 확인)** | 여러 행 가능        | ❌ 에러 없음      |
| `a = (SELECT x FROM ...)`          | **스칼라 서브쿼리**              | 1행 1열만         | ✅ 여러 행이면 에러  |


* 예시
 * 하위 쿼리 결과가 2행 이상 나오면 에러
 * 음 그러면 WHERE 값이 무조건 고유값 or LIMIT = 1을 걸어둬야겠고만
~~~sql
SELECT * FROM t1
WHERE (col1, col2) = (
  SELECT col3, col4 FROM t2 WHERE id = 10
);

SELECT * FROM t1
WHERE (col1, col2) = (SELECT col3, col4 FROM t2 WHERE id 10);
~~~

### 15.2.15.6 Subqueries with EXISTS or NOT EXISTS
* 기본 개념
* 하위 쿼리가 하나라도 행을 반환하면
 * → EXISTS 조건은 TRUE

* 하위 쿼리가 아무 행도 반환하지 않으면
 * → EXISTS는 FALSE, NOT EXISTS는 TRUE
~~~sql
SELECT column1 FROM t1 WHERE EXISTS (SELECT * FROM t2);
~~~
* 하위 쿼리에서 SELECT * 대신 SELECT 5, SELECT column1 등 아무거나 써도 상관없습니다.
* MySQL은 SELECT 리스트(컬럼들)를 무시하고 오직 “행이 존재하는가”만 판단합니다.

* 예시 1. 하나 이상의 도시에 있는 매장 유형은?
~~~sql
SELECT DISTINCT store_type FROM stores
WHERE EXISTS (
  SELECT * FROM cities_stores
  WHERE cities_stores.store_type = stores.store_type
);

* 예시 2. 어느 도시에도 없는 매장 유형은?
~~~sql
SELECT DISTINCT store_type FROM stores
WHERE NOT EXISTS (
  SELECT * FROM cities_stores
  WHERE cities_stores.store_type = stores.store_type
);
~~~

* 예시 3.  모든 도시에 존재하는 매장 유형은? NOT EXISTS 이중 사용
~~~sql
SELECT DISTINCT store_type FROM stores
WHERE NOT EXISTS (
  SELECT * FROM cities
  WHERE NOT EXISTS (
    SELECT * FROM cities_stores
    WHERE cities_stores.city = cities.city
    AND cities_stores.store_type = stores.store_type
  )
);
~~~
* stores 테이블에서 store_type의 고유값 출력할게 (돌아가면서 평가)
 * 조건1: 대신 cities 테이블에서 city값이 출력되지 않는다면
 * 조건2: 근데 이 city값은 citiesstores에서 city값과 store_type이 없으면 출력되긴 해
 * 의도: 조건2의 값이 없어야 조건1이 출력됨 = 모든 city에 해당 유형이 잆다는 것

### 15.2.15.7 Correlated Subqueries
* 기본 개념
 * 상관 서브쿼리는 서브쿼리 안에서 외부 쿼리의 테이블 값을 참조하는 서브쿼리
 * 예시 문제에서는 t1이 서브쿼리 내 참조하는 테이블이 되고 있음 (서브 쿼리에서는 t2가 기준 테이블이기에)
~~~sql
SELECT * 
FROM t1 
WHERE column1 = ANY (
    SELECT column1 FROM t2 
    WHERE t2.column2 = t1.column2
);
~~~

* 스코프 규칙 (Scoping Rule)
~~~sql
SELECT column1 FROM t1 AS x
WHERE x.column1 = (
  SELECT column1 FROM t2 AS x
  WHERE x.column1 = (
    SELECT column1 FROM t3
    WHERE x.column2 = t3.column1
  )
)
~~~
* 여기서 x는 가장 가까운 범위에서 우선 적용돼.
* 그래서 x.column2는 t2의 컬럼이지, t1의 컬럼이 아님. (왜냐면 안쪽 서브쿼리에서 t2에 x라는 별칭을 다시 줬기 때문)

* MySQL 8.0.24부터 가능한 최적화
~~~sql
SELECT * FROM t1 
WHERE (
  SELECT a FROM t2 
  WHERE t2.a = t1.a
) > 0;
~~~
* t1의 모든 행마다 서브쿼리를 매번 실행해야 함 → 느림!

~~~sql
SELECT t1.* 
FROM t1 
LEFT OUTER JOIN (
  SELECT a, COUNT(*) AS ct 
  FROM t2 
  GROUP BY a
) AS derived
ON t1.a = derived.a
AND REJECT_IF(ct > 1, "ERROR 1242")
WHERE derived.a > 0;
~~~

| 조건              | 설명                                        |
| --------------- | ----------------------------------------- |
| 서브쿼리 위치         | SELECT, WHERE, HAVING 가능 / JOIN 조건에는 불가   |
| LIMIT / OFFSET  | 있으면 변환 불가                                 |
| 집합 연산 (UNION 등) | 있으면 안 됨                                   |
| WHERE 조건        | AND로만 결합 / OR 있으면 안 됨                     |
| WHERE 조건 형태     | 반드시 `=`, 그리고 양쪽 모두 단순한 컬럼 참조만 가능해야        |
| 상관 컬럼 사용 위치     | WHERE 절 안에서만 허용 (SELECT, GROUP BY 등에서는 ❌) |
| 서브쿼리 안에 집계 함수   | 외부 블록 참조하면 ❌                              |
| COUNT()         | 반드시 맨 바깥쪽 SELECT 리스트 안에만 허용               |



# 2. CTE

~~~
✅ 학습 목표 :
* CTE에 대한 문법을 이해하고 활용할 수 있다. 
~~~
> 기본 개념
* 공통 테이블 표현식(CTE)은 단일 명령문의 범위 내에 존재하는 명명된 임시 결과 집합으로, 나중에 해당 명령문 내에서 여러 번 참조될 수 있습니다. 다음 설명에서는 CTE를 사용하는 명령문을 작성하는 방법을 설명
* **CTE (Common Table Expression)**는 쿼리 내에서 임시로 정의되는 이름 있는 서브쿼리.
* WITH 절을 사용하여 선언하며, 한 쿼리 내에서 여러 번 참조할 수 있음.
* 쿼리의 가독성을 높이고, 복잡한 쿼리를 나누어 작성할 수 있도록 도와줌.

> 공통 테이블 표현식 (Common Table Expressions)

~~~sql
WITH
  cte1 AS (SELECT a, b FROM table1),
  cte2 AS (SELECT c, d FROM table2)
SELECT b, d FROM cte1 JOIN cte2
WHERE cte1.a = cte2.c;
~~~

* WITH 절 구성
~~~sql
WITH [RECURSIVE]
  cte_name [(col_name [, col_name] ...)] AS (subquery)
  [, cte_name [(col_name [, col_name] ...)] AS (subquery)] ...
~~~

* 열 이름 명시 방식
~~~sql
WITH cte (col1, col2) AS (
  SELECT 1, 2
)
SELECT col1, col2 FROM cte;
~~~
* 열 이름은 테이블 이름 뒤에 정의해야 함

* 사용 가능 위치 (Context)
 * 주요 SQL 문에서 맨 앞에 위치할 수 있습니다:
 * 하위 쿼리에서도 사용 가능
 * SELECT를 포함하는 문에서도 사용 가능:2

* CTE 정의 시 주의사항
1. 동일 수준에서는 WITH 절을 한 번만 사용할 수 있습니다.
❌ 잘못된 예시:
~~~sql
WITH cte1 AS (...) WITH cte2 AS (...) SELECT ...
~~~
✅ 올바른 예시:
~~~sql
WITH cte1 AS (...), cte2 AS (...) SELECT ...
~~~

2. 이름은 WITH 절 내에서 서로 달라야 합니다.

❌ 오류:
~~~sql
WITH cte1 AS (...), cte1 AS (...) SELECT ...
~~~
✅ 수정:
~~~sql
WITH cte1 AS (...), cte2 AS (...) SELECT ...
~~~

3. 다른 CTE를 참조할 때는 순서를 지켜야 합니다.
~~~sql
cte1 → cte2 → cte1 (순환 참조 불가)
~~~

* 스코프(scope) 및 이름 해석
쿼리 블록 내부에서는 Derived Table > CTE > 실제 테이블 순으로 이름 우선순위가 정해짐

예를 들어 같은 이름의 CTE와 테이블이 있다면, CTE가 우선됨


<br>

<br>

---

# 확인문제

## 문제 1

> **🧚예린이는 최근 여러 주문 데이터를 분석하는 업무를 맡게 되었습니다. 특정 고객의 주문 이력을 분석하기 위해, 다음과 같이 최근 30일간 주문만 필터링한 CTE를 사용해 쿼리를 작성했습니다.**

~~~sql
WITH RecentOrders AS (
  SELECT *
  FROM Orders
  WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
)
SELECT customer_id, COUNT(*) AS recent_order_count
FROM RecentOrders
GROUP BY customer_id;
~~~

> **그런데 예린이는 "이 쿼리를 WITH 없이, 서브쿼리 방식으로 바꿔서 실행해보라" 는 피드백을 받았고, 서브쿼리로 작성해보려 했지만 익숙하지 않아 SQL_ADVANCED를 듣는 학회원분들에게 도움을 요청하고 있습니다. 예린이의 쿼리를 WITH 없이 서브쿼리로 변환해보세요. 그리고 두 방식의 차이점을 설명해보고, 각각의 장단점을 정리해보세요**



~~~sql
SELECT customer_id, COUNT(*) AS recent_order_count
FROM (
  SELECT customer_id
  FROM Orders
  WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
) AS RecentOrders
GROUP BY customer_id;
~~~





### 🎉 수고하셨습니다.
