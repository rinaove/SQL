# SQL_ADVANCED 1주차 정규 과제 

## Week 1 :집합 연산자 & 그룹 함수

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스/ Solvesql / LeetCode에서 SQL 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_1st_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**(수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 

## SQL_ADVANCED_1st

**1. 집합 연산자**

### 15.2.18. UNION Clause

### 15.2.14. Set Operations with UNION, INTERSECT

- UNION, UNION ALL 중심으로 개념을 정리하고, INTERSECT, EXCEPT는 구문이 어떤 기능을 하는지 간단히만 알아봅니다. EXCEPT와 INTERSECT는 대부분 MySQL 버전에서 공식 지원되지 않기 때문에, **이번주 학습은 `UNION, UNION ALL` 만 집중적으로 정리해주세요.**

**2. 그룹 함수 (집계 함수)**

### 14.19.1. Aggregate Function Descriptions





### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**





> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - 15.2.18. UNION Clause : MySQL 공식문서 
>
> https://dev.mysql.com/doc/refman/8.0/en/union.html
>
> (한국어 버전)
>
> - 15.2.14. Set Operations with UNION, INTERSECT : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/set-operations.html
>
> (한국어 버전)
>
> - 14.19.1. Aggregate Function Descriptions : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html
>
> (한국어 버전)
>



## 🏁 주차별 학습 (Study Schedule)

| 주차  | 공부 범위               | 완료 여부 |
| ----- | ----------------------- | --------- |
| 0주차 | 서브쿼리 & CTE          | ✅         |
| 1주차 | 집합 연산자 & 그룹 함수 | ✅         |
| 2주차 | 윈도우 함수             | 🍽️         |
| 3주차 | Top N 쿼리              | 🍽️         |
| 4주차 | 계층형 질의와 셀프 조인 | 🍽️         |
| 5주차 | PIVOT / UNPIVOT         | 🍽️         |
| 6주차 | 정규 표현식             | 🍽️         |





## 문제

https://leetcode.com/problems/customers-who-never-order/

> LeetCode 183. Customers Who never Order
>
> 학습 포인트 : 주문 내역이 없는 고객을 찾기 위한 패턴 익히기  

https://leetcode.com/problems/department-highest-salary/description/

> LeetCode 184. Department Highest Salary
>
> 학습 포인트 : 부서별 최고 연봉자 추출을 위한 **그룹별 정렬 / 필터링** 방식 이해하기





<!-- 여기까진 그대로 둬 주세요-->

---

 # 1. 집합 연산자

~~~
✅ 학습 목표 :
* UNION과 UNION ALL의 차이와 사용법을 이해한다.
* 중복 제거 여부, 컬럼 정렬 조건 등을 고려하여 올바르게 집합 연산자를 사용할 수 있다. 
~~~

### 15.2.18. UNION Clause
- ✅ **UNION 절이란?**
  - UNION은 여러 개의 SELECT 쿼리 결과를 하나의 결과 집합으로 합치는 SQL 연산자
  - 기본적으로 중복은 제거됨
- ✅ **사용 조건**
  - SELECT절의 컬럼 수와 순서가 같아야 함
  - 각 SELECT의 열 데이터 타입이 호환되어야 함
  - ORDER BY는 마지막 SELECT 이후에만 사용 가
- ✅ **기본 문법**
  ```sql
  SELECT column1, column2 FROM table1
  UNION
  SELECT column1, column2 FROM table2;
  ```
- ✅ **예시**
  -  1, 2와 'a', 'b'는 데이터 타입이 다르지만 MySQL이 자동으로 모두 문자열로 형 변환합니다.
  -  열 이름은 첫 번째 SELECT의 결과에서 따릅니다.
  -  기본적으로 UNION은 중복 제거지만, 여기서는 중복이 없으므로 두 행이 모두 출력됩니다.
    ```sql
      mysql> SELECT 1, 2;
    +---+---+
    | 1 | 2 |
    +---+---+
    | 1 | 2 |
    +---+---+
    mysql> SELECT 'a', 'b';
    +---+---+
    | a | b |
    +---+---+
    | a | b |
    +---+---+
    mysql> SELECT 1, 2 UNION SELECT 'a', 'b';
    +---+---+
    | 1 | 2 |
    +---+---+
    | 1 | 2 |
    | a | b |
    +---+---+
    ```
### 15.2.14. Set Operations with UNION, INTERSECT
- ✅ **SQL 집합 연산자**
  - SQL 집합 연산자는 여러 SELECT, TABLE, 또는 VALUES 문(= 쿼리 블록)의 결과를 하나의 단일 결과 집합으로 결합하는 연산
    
  | 연산자         | 설명                                  |
  | ----------- | ----------------------------------- |
  | `UNION`     | 두 쿼리의 결과를 결합하고 **중복 제거**            |
  | `INTERSECT` | 두 쿼리 결과의 **공통된 행만 반환**              |
  | `EXCEPT`    | 첫 번째 쿼리의 결과 중 **두 번째 쿼리에 없는 행만 반환** |

- ✅ **ALL, DISTINCT 키워드**
  - `DISTINCT` (기본값): 결과에서 중복 제거
  - `ALL`: 중복을 제거하지 않고 모두 출력 (성능 ↑, 정확성 ↓ 가능)

- ✅ **지원되는 쿼리 블록 종류**
  - 집합 연산자는 다음 쿼리 블록들과 함께 사용 가능합니다:

  | 구문 유형    | 설명                          |
  | -------- | --------------------------- |
  | `SELECT` | 일반 SELECT 문                 |
  | `TABLE`  | 테이블 전체 조회 (MySQL 8.0.19+)   |
  | `VALUES` | 명시적 값 목록 반환 (MySQL 8.0.19+) |

- ✅ **연산 우선순위**
  - INTERSECT는 UNION 또는 EXCEPT보다 먼저 평가됩니다.
  ```sql
  TABLE x UNION TABLE y INTERSECT TABLE z
  -- 실제 계산 순서:
  TABLE x UNION (TABLE y INTERSECT TABLE z)
  ```
- ✅ **결합 순서와 가환성**

  | 연산자         | 가환성 (순서 상관 없음) | 설명                                            |
  | ----------- | -------------- | --------------------------------------------- |
  | `UNION`     | ✅ O            | `A UNION B` = `B UNION A` (단, 정렬 순서는 다를 수 있음) |
  | `INTERSECT` | ✅ O            | `A INTERSECT B` = `B INTERSECT A`             |
  | `EXCEPT`    | ❌ X            | `A EXCEPT B` ≠ `B EXCEPT A`                   |

- ✅ **구문 유형 **
  ```sql
  query_block [set_op query_block] [set_op query_block] ...

  query_block:
      SELECT ...
    | TABLE table_name
    | VALUES ROW(...)
  
  set_op:
      UNION | INTERSECT | EXCEPT
  ```
- ✅ **구문 유형2**
  ```sql
  WITH active_users AS (
  SELECT name FROM users WHERE status = 'active'
  ),
  newsletter_users AS (
    SELECT name FROM subscribers WHERE subscribed = 1
  )
  SELECT * FROM active_users
  INTERSECT
  SELECT * FROM newsletter_users
  ORDER BY name
  LIMIT 5;
  ```
- 1️⃣ **결과 집합 열 이름 및 데이터 유형**
  - 집합 연산 결과의 열 이름은 첫 번째 쿼리 블록의 열 이름에서 가져옵니다
  - 열 개수는 같아야 함: 모든 SELECT 문에서 선택하는 열의 수가 동일해야 합니다.
  - 각 열의 데이터 타입이 일치하거나 호환되어야 함
    - 첫 번째 SELECT의 1열이 VARCHAR라면, 다른 SELECT의 1열도 VARCHAR거나 문자열로 암묵적 변환이 가능해야 함.



- 2️⃣ **TABLE 및 VALUES 문을 사용한 집합 연산**
  - TABLE 테이블명 → SELECT * FROM 테이블 과 같은 역할을 함
  - VALUES ROW(값1, 값2) → 테이블 없이 직접 값을 생성하는 방법
  - 열 이름 맞추기 (표시되는 헤더 바꾸기)
    ```sql
    SELECT * FROM (TABLE t2) AS t(x, y)
    UNION
    TABLE t1;
    ```
- 3️⃣ **DISTINCT 및 ALL을 사용한 집합 연산**
  - 기본 동작: 중복 제거 
  - ALL 키워드: 중복 유지
  - DISTINCT 키워드: 명시적 중복 제거
  - 집합 연산자를 섞어 쓸 수 있으며, 다음 규칙이 적용됩니다:
    - 왼쪽에 있는 연산자의 성격이 우선
    - 즉, ALL → DISTINCT 구조에서는 중복이 제거됨

- 4️⃣ **ORDER BY 및 LIMIT을 사용한 작업 설정**
  ```sql
  (SELECT a FROM t1 WHERE a=10 AND b=1 ORDER BY a LIMIT 10)
  UNION
  (SELECT a FROM t2 WHERE a=11 AND b=2 ORDER BY a LIMIT 10);
  
  (TABLE t1 ORDER BY x LIMIT 10) 
  INTERSECT 
  (TABLE t2 ORDER BY a LIMIT 10);
  ```
  - ORDER BY에서는 열 이름이나 별칭만 사용 가능

- 5️⃣ **UNION과 UNION ALL 차이**

  | 항목       | `UNION`             | `UNION ALL`             |
  | -------- | ------------------- | ----------------------- |
  | 중복 제거    | ✅ **제거함**           | ❌ **제거하지 않음**           |
  | 성능       | 느릴 수 있음 (중복 검사 필요)  | 빠름 (그냥 붙이기만 함)          |
  | 기본 사용 목적 | **중복 없는 결과가 필요할 때** | **모든 데이터 그대로 볼 때**      |
  | 정렬 순서    | 기본은 무작위             | 무작위 (둘 다 `ORDER BY` 필요) |


# 2. 그룹함수

~~~
✅ 학습 목표 :
* COUNT, SUM, AVG, MAX, MIN 함수의 기본 사용법을 익힌다.
* GROUP BY와 HAVING 절을 적절히 활용할 수 있다.
* NULL과 집계 함수가 어떻게 상호작용하는지 이해한다. 
~~~

### 14.19.1 Aggregate Function Descriptions

| 함수 이름              | 설명 (한국어)                   |
| ------------------ | -------------------------- |
| `AVG()`            | 인자의 평균 값을 반환               |
| `BIT_AND()`        | 비트 단위 AND 결과 반환            |
| `BIT_OR()`         | 비트 단위 OR 결과 반환             |
| `BIT_XOR()`        | 비트 단위 XOR 결과 반환            |
| `COUNT()`          | 반환된 행의 개수 계산               |
| `COUNT(DISTINCT)`  | 서로 다른 값의 개수를 계산            |
| `GROUP_CONCAT()`   | 그룹의 값을 하나의 문자열로 연결하여 반환    |
| `JSON_ARRAYAGG()`  | 결과 집합을 단일 JSON 배열로 반환      |
| `JSON_OBJECTAGG()` | 결과 집합을 단일 JSON 객체로 반환      |
| `MAX()`            | 최대값 반환                     |
| `MIN()`            | 최소값 반환                     |
| `STD()`            | 모집단 표준편차 반환 (`STDDEV`과 동일) |
| `STDDEV()`         | 모집단 표준편차 반환 (`STD`와 동일)    |
| `STDDEV_POP()`     | 모집단 표준편차 반환                |
| `STDDEV_SAMP()`    | 표본 표준편차 반환                 |
| `SUM()`            | 합계 반환                      |
| `VAR_POP()`        | 모집단 분산 반환                  |
| `VAR_SAMP()`       | 표본 분산 반환                   |
| `VARIANCE()`       | 모집단 분산 반환 (`VAR_POP`과 동일)  |


- ✅ 공통 규칙
  1. NULL 무시
  대부분의 집계 함수는 NULL 값을 무시하고 연산합니다. 단, 모든 값이 NULL이면 결과도 NULL입니다.
  예: AVG(NULL, NULL) → NULL, SUM(10, NULL) → 10
  
  2. GROUP BY 없는 집계 함수
  GROUP BY가 없는 상태에서 집계 함수 사용 시, 전체 테이블을 하나의 그룹으로 간주합니다.
  
  3. 윈도우 함수로 사용 가능
  OVER() 절이 붙으면 윈도우 함수로 작동합니다. 단, 일부 함수는 DISTINCT와 OVER()를 함께 사용할 수 없습니다.
  
  4. 데이터 타입별 반환값
  VAR_POP, STD, STDDEV 등의 분산 및 표준편차 함수는 항상 DOUBLE을 반환합니다.
  
  5. 시간 데이터에 대한 처리
  SUM()이나 AVG()는 시간 형식 데이터(TIME, DATE)에 직접 사용하면 안 됩니다.
  ```sql
  SELECT SEC_TO_TIME(SUM(TIME_TO_SEC(time_col))) FROM tbl_name;
  SELECT FROM_DAYS(SUM(TO_DAYS(date_col))) FROM tbl_name;
  ```

- 1️⃣ COUNT(expr) [over_clause]
  - expr이 NULL이 아닌 행의 개수를 반환합니다.
  - 일치하는 행이 없음 0 반환
  - 윈도우 함수 사용 가능

- 2️⃣ AVG([DISTINCT] expr) [over_clause]
  - DISTINCT옵션을 사용하면 의 고유 값의 평균을 반환
  - NULL. 또한 이 함수는 를 NULL 반환
  - AVG()는 OVER() 절과 함께 윈도우 함수로 사용 가능

- 3️⃣ MAX([DISTINCT] expr) [OVER (...)]
  - 문자열 인수를 받을 수 있으며, 이 경우 최대 문자열 값을 반환
  - DISTINCT를 붙이면, 중복을 제거한 값들 중 최대값을 구함
  -  결과 행이 하나도 없거나 expr가 모두 NULL이면, 결과는 NULL
  -  OVER() 절을 통해 윈도우 함수로도 사용할 수 있음
 
    ```sql
    SELECT student_name, MIN(test_score), MAX(test_score)
    FROM student
    GROUP BY student_name;
    ```

- 4️⃣ MIN([DISTINCT] expr) [over_clause]
  ```sql
  SELECT student_name, MIN(test_score), MAX(test_score)
  FROM student
  GROUP BY student_name;
  ```



<br>

<br>

---

# 확인문제

## 문제 1

> **🧚동혁이는 SQL 문제를 풀면서 `UNION과 UNION ALL`의 차이를 명확히 이해하지 못해 중복된 값이 생기거나 누락되는 문제를 계속 겪고 있습니다.** 아래는 동혁이가 작성한 쿼리입니다.

~~~sql
SELECT name FROM member
UNION
SELECT name FROM blackList;
~~~

> **그런데 예상과 달리 blacklist에만 있는 이름이 결과에 안 나오거나, 중복된 이름이 사라져서 헷갈리고 있습니다. UNION과 UNION ALL의 차이를 설명하고, 중복 포함 여부에 따라 어떤 경우에 어떤 쿼리를 써야 하는지 예시와 함께 설명해주세요**

<br>

~~~
여기에 답을 작성해주세요!
~~~



<br>

### 🎉 수고하셨습니다.
