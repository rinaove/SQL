# SQL_ADVANCED 3주차 정규 과제 

## Week 3 : TOP-N 쿼리

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스, SolveSql, LeetCode 중에서 SQL 문제 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_3rd_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**(수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 



## SQL_ADVANCED_3rd

### 13.2.10. SELECT Statement

- `ORDER BY, LIMIT, LIMIT and Subqueries` 중심으로 학습해주세요. 



### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**





> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - 13.2.10. SELECT Statement : MySQL 공식문서 
>
> https://dev.mysql.com/doc/refman/8.0/en/select.html#order-by-optimization
>
> (한국어 버전)
>





## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위               | 완료 여부 |
| ----- | ----------------------- | --------- |
| 0주차 | 서브쿼리 & CTE          | ✅         |
| 1주차 | 집합 연산자 & 그룹 함수 | ✅         |
| 2주차 | 윈도우 함수             | ✅         |
| 3주차 | Top N 쿼리              | ✅         |
| 4주차 | 계층형 질의와 셀프 조인 | 🍽️         |
| 5주차 | PIVOT / UNPIVOT         | 🍽️         |
| 6주차 | 정규 표현식             | 🍽️         |



<br>



## 문제 

https://leetcode.com/problems/rank-scores/

> LeetCode 178. Rank Scores
> <img width="1413" height="508" alt="image" src="https://github.com/user-attachments/assets/098f3381-244a-4457-80b8-6880d693aabd" />
> 학습 포인트 : DENSE_RANK( )를 활용하여 점수별 순위 부여, 동점자 처리, 윈도우 함수 복습 

https://school.programmers.co.kr/learn/courses/30/lessons/133027

> 프로그래머스 : 주문량이 많은 아이스크림들 조회하기 (Lev 4)
>
> Hint
>
> - 문제 핵심은 '총 주문량 합산' 입니다. 
> <img width="1439" height="463" alt="image" src="https://github.com/user-attachments/assets/12df73db-3404-4360-90ff-12c0b8e40704" />
> - 두 테이블을 '세로로' 합쳐야합니다. 
>   - 저희는 이 부분을 1주차에 `UNION ALL` 을 통해 방법을 배웠습니다. 
> - 합쳐진 테이블에서 FLAVOR 별로 그룹화해 주문량을 합산하셍.
> - 상위 3개를 추출 = 주문량 기준으로 내림차순하여 이번에 학습한 것을 사용해야 합니다. 





<!-- 여기까진 그대로 둬 주세요-->

---

 # 1. TOP N 쿼리

~~~
✅ 학습 목표 :
* LIMIT 와 ORDER BY를 이용한 TOP-N 쿼리 작성이 가능하다.
* SubQuery나 RANK 대신 LIMIT으로 간단한 순위 집계가 가능함을 이해한다. 
~~~

### 15.2.13 SELECT Statement
✅ MySQL SELECT 실행 순서

| 처리 순서 | 절 이름       | 설명                    |
| ----- | ---------- | --------------------- |
| 1️⃣   | `FROM`     | 어떤 테이블에서 데이터를 가져올지 결정 |
| 2️⃣   | `WHERE`    | 조건에 맞는 **행(row)** 필터링 |
| 3️⃣   | `GROUP BY` | 특정 컬럼으로 그룹핑 (있다면)     |
| 4️⃣   | `HAVING`   | 그룹핑된 결과를 다시 필터링       |
| 5️⃣   | `SELECT`   | 어떤 컬럼을 보여줄지 결정        |
| 6️⃣   | `ORDER BY` | 정렬 수행 (이때 별칭도 참조 가능)  |
| 7️⃣   | `LIMIT`    | 반환할 행 수 제한            |

✅ LIMIT 절 설명
- LIMIT은 SELECT 결과에서 반환할 행 수 또는 행 범위(offset 포함) 를 제한하는 데 사용
- LIMIT이 서브쿼리 안에 있고, 외부 쿼리에 영향을 미치는 경우, 결과는 정의되지 않음
| 구문                              | 설명                                   |
| ------------------------------- | ------------------------------------ |
| `LIMIT row_count`               | 결과의 **앞에서부터 `row_count`개** 반환        |
| `LIMIT offset, row_count`       | **`offset + 1`번째부터 `row_count`개** 반환 |
| `LIMIT row_count OFFSET offset` | 위와 같은 의미 (PostgreSQL 호환 문법)          |

```sql
SELECT * FROM tbl LIMIT 5, 10; -- 6,10행
```

✅ LIMIT + ORDER BY를 이용한 TOP-N 쿼리
```sql
SELECT *
FROM your_table
ORDER BY some_column DESC
LIMIT N;
```

✅ SubQuery나 RANK() 대신 LIMIT으로 순위 집계 가능
```sql
SELECT name, sales
FROM employees
ORDER BY sales DESC
LIMIT 1;
```

✅ 언제 LIMIT으로 충분하고, 언제 RANK()가 필요한가?
| 상황                      | 추천 방법                                           |
| ----------------------- | ----------------------------------------------- |
| 상위 N개만 필요할 때            | `ORDER BY` + `LIMIT`                            |
| 순위 자체(등수)를 보여줘야 할 때     | `RANK()` 같은 윈도우 함수                              |
| 공동 순위 (동점자)까지 포함하고 싶을 때 | `RANK()` 또는 `DENSE_RANK()`                      |
| 범위 필터링 (ex: 2\~5등)      | `LIMIT offset, count` 또는 `RANK()`의 `WHERE` 절 사용 |



<br>

<br>

---

# 확인문제

## 문제 1

> **🧚미정이는 지역별로 가장 인기 있는 식당 2곳씩을 뽑기 위해 다음과 같은 UNION ALL 기반 쿼리를 작성했습니다.**

~~~sql
(
  SELECT region, restaurant_name, review_count
  FROM Restaurants
  WHERE region = '서울'
  ORDER BY review_count DESC
  LIMIT 2
)
UNION ALL
(
  SELECT region, restaurant_name, review_count
  FROM Restaurants
  WHERE region = '부산'
  ORDER BY review_count DESC
  LIMIT 2
)
UNION ALL
(
  SELECT region, restaurant_name, review_count
  FROM Restaurants
  WHERE region = '대구'
  ORDER BY review_count DESC
  LIMIT 2
);
~~~

> **쿼리는 잘 작동하긴 하지만, 지역을 더 추가해달라는 권택이의 부탁으로 UNION ALL 블록을 계속 추가하게 되어 관리가 어려울 것 같아서 힘들어하고 있었습니다. 여러분들은 이 쿼리를 윈도우 함수로 변경하여 더 쉽게 리팩토링을 하려고 합니다. 미정이를 도와서 UNION ALL 없이 RANK( ) 또는 ROW_NUMBER( ) 윈도우 함수를 사용해, 각 지역별로 리뷰 수가 가장 많은 상위 2개 식당을 추출하는 쿼리를 작성해보세요.**
,

~~~sql
SELECT region, restaurant_name, review_count
FROM (
  SELECT region, restaurant_name, review_count,
         ROW_NUMBER() OVER (PARTITION BY region ORDER BY review_count DESC) AS rn
  FROM Restaurants
) ranked
WHERE rn <= 2;
~~~



<br>

### 🎉 수고하셨습니다.
