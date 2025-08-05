# SQL_ADVANCED 2주차 정규 과제 

## Week 2 : 윈도우 함수 (Window Functions)

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 이번 주차에는 LeetCode SQL 문제 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_2nd_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**(수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 

## SQL_ADVANCED_2nd

### 14.20.2 Window Function Concepts and Syntax

### 14.20.1 Window Function Description

### 14.19.1 Aggregate Function Descriptions

- 위 문서 중 중복되는 함수 설명은 1주차 (집계 함수) 에서 다뤘기 때문에, 이번 주차에서는 **OVER( ) 절을 활용한 윈도우 함수 문법과 `RANK( ), DENSE_RANK( ), ROW_NUMBER( ), LAG( ), LEAD( )`등 윈도우 함수 특유의 기능 중심으로 정리해주세요.**



### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**





> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - 14.20.2 Window Function Concepts and Syntax : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html
>
> (한국어 버전)
>
> - 14.20.1 Window Function Description : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html
>
> (한국어 버전)
>
> - 14.19.1 Aggregate Function Descriptions : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html
>
> (한국어 버전)
>



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위               | 완료 여부 |
| ----- | ----------------------- | --------- |
| 0주차 | 서브쿼리 & CTE          | ✅         |
| 1주차 | 집합 연산자 & 그룹 함수 | ✅         |
| 2주차 | 윈도우 함수             | ✅         |
| 3주차 | Top N 쿼리              | 🍽️         |
| 4주차 | 계층형 질의와 셀프 조인 | 🍽️         |
| 5주차 | PIVOT / UNPIVOT         | 🍽️         |
| 6주차 | 정규 표현식             | 🍽️         |

<br>



## LeetCode 문제 

https://leetcode.com/problems/department-top-three-salaries/

> LeetCode 185. Department Top Three Salaries 
>
> 학습 포인트 : DENSE_RANK( ) + PARTITION BY 사용으로 그룹 내 상위 N개 추출

https://leetcode.com/problems/consecutive-numbers/

> LeetCode 180. Consecutive Numbers 
>
> 학습 포인트 : LAG( ) 함수로 이전 값과 비교하여 연속 데이터 탐지 

https://leetcode.com/problems/last-person-to-fit-in-the-bus/

> LeetCode 2481. Last Person to Fit in the Bus 
>
> 학습 포인트 : SUM( ) OVER (ORDER BY ...) 로 누적 합계 계산 후 조건 필터링 



문제를 푸는 다양한 방법이 존재하지만, **윈도우 함수를 사용하여 해결하는 방식에 대해 고민해주시길 바랍니다.** 



<!-- 여기까진 그대로 둬 주세요-->

---

 # 1. 윈도우 함수

~~~
✅ 학습 목표 :
* OVER 절을 통해 행 단위 분석을 가능하게 하는 윈도우 함수의 구조를 이해한다.
* RANK, DENSE_RANK, ROW_NUMBER의 차이를 구분하고 사용할 수 있다.
* 이전 또는 이후 행을 참조하는 LAG, LEAD 함수를 적절히 사용할 수 있다.
~~~

### 14.20.2 Window Function Concepts and Syntax
✅ 윈도우 함수(Window Function): 
- SQL에서 행(row) 단위로 계산을 수행하되, 그 행과 관련된 다른 행들을 함께 고려해 결과를 반환하는 함수.
- 집계 함수(예: SUM(), AVG() 등)와 비슷하지만, 결과를 그룹으로 묶지 않고 원래의 행 수를 그대로 유지한다는 점에서 다름.
- 행 수 그대로 유지되면서 정보가 더 풍부해짐

✅ 기본 구조
```sql
함수명(컬럼명) OVER (
  [PARTITION BY 분할기준]
  [ORDER BY 정렬기준]
  [ROWS BETWEEN 범위]
)
```
```sql
SELECT
  year, country, product, profit,
  SUM(profit) OVER() AS total_profit,
  SUM(profit) OVER(PARTITION BY country) AS country_profit
FROM sales
ORDER BY country, year, product, profit;
```

✅ OVER() 절의 역할
- OVER()는 윈도우 함수가 어느 행들에 대해 계산할지를 정하는 부분으로 이 안에 아무것도 없으면 = 전체 데이터를 대상으로 계산.
- 윈도우 함수는 SELECT와 ORDER BY에서만 사용 가능
- AVG(), SUM() 같은 집계 함수들은 OVER 절을 붙이면 윈도우 함수로도 사용할 수 있고, 붙이지 않으면 일반 집계 함수로 사용

📍 윈도우 전용 함수 예시
| 함수                               | 의미                 |
| -------------------------------- | ------------------ |
| `ROW_NUMBER()`                   | 각 행에 순번 붙이기        |
| `RANK()`                         | 동순위 허용한 순위         |
| `DENSE_RANK()`                   | 건너뛰지 않는 순위         |
| `LEAD()` / `LAG()`               | 다음 행 / 이전 행 값 가져오기 |
| `FIRST_VALUE()` / `LAST_VALUE()` | 그룹 내 첫 번째 / 마지막 값  |
| `NTILE(n)`                       | 그룹을 n개로 나누기        |


- 기본적으로 PARTITION BY로 나뉜 그룹(파티션) 안의 행들은 정렬되지 않기 때문에 순번(ROW_NUMBER)은 무작위로 매겨질 수 있음.
- 정해진 순서로 번호를 매기고 싶다면 OVER() 절 안에 ORDER BY를 꼭 써야 함.

```sql
SELECT
  year, country, product, profit,
  ROW_NUMBER() OVER(PARTITION BY country) AS row_num1, -- 정렬 미포함, 순위 무작위
  ROW_NUMBER() OVER(PARTITION BY country ORDER BY year, product) AS row_num2 -- 정렬 포함 
FROM sales;
+------+---------+------------+--------+----------+----------+
| year | country | product    | profit | row_num1 | row_num2 |
+------+---------+------------+--------+----------+----------+
| 2000 | Finland | Computer   |   1500 |        2 |        1 |
| 2000 | Finland | Phone      |    100 |        1 |        2 |
| 2001 | Finland | Phone      |     10 |        3 |        3 |
| 2000 | India   | Calculator |     75 |        2 |        1 |
| 2000 | India   | Calculator |     75 |        3 |        2 |
| 2000 | India   | Computer   |   1200 |        1 |        3 |
| 2000 | USA     | Calculator |     75 |        5 |        1 |
| 2000 | USA     | Computer   |   1500 |        4 |        2 |
| 2001 | USA     | Calculator |     50 |        2 |        3 |
| 2001 | USA     | Computer   |   1500 |        3 |        4 |
| 2001 | USA     | Computer   |   1200 |        7 |        5 |
| 2001 | USA     | TV         |    150 |        1 |        6 |
| 2001 | USA     | TV         |    100 |        6 |        7 |
+------+---------+------------+--------+----------+----------+
```

✅ OVER() 절의 형태
```sql
{OVER (window_spec) | OVER window_name}
```

- OVER(window_spec)
  - OVER() 괄호 안에 바로 PARTITION BY나 ORDER BY를 직접 작성
```sql
ROW_NUMBER() OVER (PARTITION BY country ORDER BY year)
```

* OVER window_name
  * WINDOW 절에서 미리 정의한 창을 OVER에 이름으로 참조
```sql
WINDOW w AS (PARTITION BY country ORDER BY year)
...
ROW_NUMBER() OVER (w)
```

✅ OVER(window_spec)의 구성
```sql
OVER (
  [window_name]
  [PARTITION BY ...]
  [ORDER BY ...]
  [frame_clause]
)
```
- MySQL은 **표현식(expr)**도 허용
  - 표준 SQL: PARTITION BY column_name 만 가능
  - MySQL: PARTITION BY HOUR(ts) 같은 함수나 계산식도 가능
 
```sql
PARTITION BY expr [, expr] ...
```

- `frame_clause`는 현재 파티션 중 일부만을 분석 대상으로 삼고 싶을 때 사용
  - ex. 가장 첫 행부터 현재 행까지를 포함해서 계산하라
```sql
SUM(sales) OVER (ORDER BY date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW))
```

### 14.20.1 Window Function Descriptions
✅ 함수 설명

| 함수명                 | 설명                                        | 예시               |
| ------------------- | ----------------------------------------- | ---------------- |
| **CUME\_DIST()**    | 누적 백분위 (현재 행이 전체 중 몇 %쯤인지)                | 상위 몇 %에 해당하는지    |
| **DENSE\_RANK()**   | **중복 순위 허용, 건너뛰지 않음**                     | 1, 2, 2, 3, 4... |
| **RANK()**          | 중복 순위 허용, **건너뜀**                         | 1, 2, 2, 4, 5... |
| **ROW\_NUMBER()**   | **무조건 고유 순번 부여**                              | 1, 2, 3, 4...    |
| **PERCENT\_RANK()** | (순위 - 1) / (총 행 수 - 1) → 백분위 비율           | 0.0 \~ 1.0 값     |
| **NTILE(n)**        | 파티션을 n개의 그룹(버킷)으로 나누고 현재 행이 몇 번째 그룹에 속하는지 | 1\~n             |
| **FIRST\_VALUE()**  | 윈도우 프레임 내 첫 번째 값                          | 앞에서 첫 값 참조       |
| **LAST\_VALUE()**   | 윈도우 프레임 내 마지막 값                           | 뒤에서 마지막 값 참조     |
| **NTH\_VALUE(n)**   | 프레임 내 n번째 값                               | 예: 3번째 값         |
| **LAG()**           | **이전 행의 값 참조 (몇 칸 전)**                        | 예: 전월 매출         |
| **LEAD()**          | **다음 행의 값 참조 (몇 칸 후)**                        | 예: 다음 달 매출       |

✅ 함수 용도 요약
| 카테고리       | 함수                                                                    |
| ---------- | --------------------------------------------------------------------- |
| 순위 관련      | `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `PERCENT_RANK()`, `NTILE()` |
| 과거/미래 값 참조 | `LAG()`, `LEAD()`                                                     |
| 특정 값 참조    | `FIRST_VALUE()`, `LAST_VALUE()`, `NTH_VALUE()`                        |
| 누적 분포      | `CUME_DIST()`                                                         |

1. `ROW_NUMBER()`
- 해당 파티션 내 현재 행의 번호를 반환
- 행 번호는 1부터 파티션 행 개수까지입니다.무조건 고유한 번호
- ORDER BY 생략 시 순번이 랜덤(비결정적)
  ```sql
  ROW_NUMBER() OVER (PARTITION BY col ORDER BY col2)
  ```

2. `RANK()`
- 값이 같으면 같은 순위, 하지만 건너뜀 있음

3. `DENSE_RANK()`
- 값이 같으면 같은 순위, 건너뛰지 않음

| 이름 | 급여  | `RANK()` | `DENSE_RANK()` |
| -- | --- | -------- | -------------- |
| A  | 100 | 1        | 1              |
| B  | 100 | 1        | 1              |
| C  | 80  | 3        | 2              |


4. `LAG(expr, N, default)`
- 현재 행 기준으로 N행 앞의 값을 반환
- 기본값: N = 1, default = NULL (따로 지정 안 했을 경우)


5. `LEAD(expr, N, default)`
- 현재 행 기준으로 N행 뒤의 값을 반환
- 기본값: N = 1, default = NULL (따로 지정 안 했을 경우)


- LAG()(및 유사 LEAD()함수)는 행 간의 차이를 계산하는 데 자주 사용
```sql
SELECT n
FROM fib
ORDER BY n;
+------+
| n    |
+------+
|    1 |
|    1 |
|    2 |
|    3 |
|    5 |
|    8 |
+------+

SELECT
  t, val,
  LAG(val)        OVER w AS 'lag',
  LEAD(val)       OVER w AS 'lead',
  val - LAG(val)  OVER w AS 'lag diff',
  val - LEAD(val) OVER w AS 'lead diff'
FROM series
WINDOW w AS (ORDER BY t);
+----------+------+------+------+----------+-----------+
| t        | val  | lag  | lead | lag diff | lead diff |
+----------+------+------+------+----------+-----------+
| 12:00:00 |  100 | NULL |  125 |     NULL |       -25 |
| 13:00:00 |  125 |  100 |  132 |       25 |        -7 |
| 14:00:00 |  132 |  125 |  145 |        7 |       -13 |
| 15:00:00 |  145 |  132 |  140 |       13 |         5 |
| 16:00:00 |  140 |  145 |  150 |       -5 |       -10 |
| 17:00:00 |  150 |  140 |  200 |       10 |       -50 |
| 18:00:00 |  200 |  150 | NULL |       50 |      NULL |
+----------+------+------+------+----------+-----------+
```

- 피보나치 수열
  - n + LAG(...): 현재 값 + 이전 값 = 다음 피보나치 수
  - n + LEAD(...): 현재 값 + 다음 값 = 다다음 수
```sql 
SELECT
  n,
  LAG(n, 1, 0)      OVER w AS 'lag',
  LEAD(n, 1, 0)     OVER w AS 'lead',
  n + LAG(n, 1, 0)  OVER w AS 'next_n',
  n + LEAD(n, 1, 0) OVER w AS 'next_next_n'
FROM fib
WINDOW w AS (ORDER BY n);
```

- 이외 활용 예시
| 활용 예        | 설명                                 |
| ----------- | ---------------------------------- |
| 시간 간격 차이 계산 | `val - LAG(val)`                   |
| 이동 평균 계산    | `(val + LAG(val) + LEAD(val)) / 3` |
| 피보나치 예측     | `val + LAG(val)`                   |

6. `이외 함수`
- `CUME_DIST()`: 누적 백분위 값 (현재 값 이하인 비율)
- `PERCENT_RANK()`: 상대적인 순위 백분율
- `NTILE(N)`: 파티션을 N개 그룹(bucket)으로 나누고, 각 행이 몇 번째 그룹에 속하는지 알려줌
- `FIRST_VALUE(expr)`: 현재 프레임의 첫 번째 값 반환
  - 왜 사용하지:  각 행마다 “내 파티션 내 첫 번째 값이 뭐였는지”를 알려주는 함수
    ```sql
    SELECT
      subject,
      time,
      score,
      FIRST_VALUE(score) OVER (
        PARTITION BY subject
        ORDER BY time
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
      ) AS first_score
    FROM exam_results;

    | subject | time     | score | first\_score |
    | ------- | -------- | ----- | ------------ |
    | math    | 09:00:00 | 85    | 85           |
    | math    | 10:00:00 | 90    | 85           |
    | math    | 11:00:00 | 95    | 85           |
    ```
- `LAST_VALUE(expr)`: 현재 프레임의 마지막 값 반환
  - 이름은 "마지막"이지만, 프레임이 어디까지 열려 있느냐에 따라 결과가 달라짐
  - ROWS BETWEEN ... 안 쓰면 보통 CURRENT ROW까지만 봐서 “진짜 마지막 값”이 아님
    ```sql
    LAST_VALUE(column) OVER (
      PARTITION BY group_col
      ORDER BY sort_col
      ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    )

    | subject | time     | score | last\_score |
    | ------- | -------- | ----- | ----------- |
    | math    | 09:00:00 | 85    | 95          |
    | math    | 10:00:00 | 90    | 95          |
    | math    | 11:00:00 | 95    | 95          |

    ```


<br>

<br>

---

# 확인문제

## 문제 1

> **🧚예린이는 고객별로 얼마나 많은 주문을 하는지 분석하기 위해, 고객의 주문 목록에 주문 순서를 표시하는 쿼리를 작성해보았습니다. 이때 주문일 순서대로 각 고객의 주문 번호를 매기기 위해 윈도우 함수를 활용했습니다.**

~~~sql
SELECT customer_id, order_id, order_date,
       ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date) AS order_rank
FROM Orders;
~~~

> **이번에는 예린이에게 "윈도우 함수를 쓰지 않고 동일한 결과를 만들어보라"는 미션을 받았습니다. 예린이는 이 작업을 어떻게 해야할지 막막합니다. 예린이를 도와 ROW_NUMBER() 윈도우 함수 없이 동일한 결과를 서브쿼리나 JOIN을 사용해서 작성해보세요.**

~~~sql 
# JOINver.
SELECT
  Od1.customer_id,
  Od1.order_id, 
  Od1.order_date, 
  COUNT(*) AS order_rank
FROM Orders as Od1 
JOIN Orders as Od2
  on Od1.customer_id = Od2.customer_id
  AND Od1.order_date >= Od2.order_date
GROUP BY customer_id
ORDER BY order_date;
~~~



<br>

### 🎉 수고하셨습니다.
