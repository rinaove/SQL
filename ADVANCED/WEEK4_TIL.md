# SQL_ADVANCED 4주차 정규 과제 

## Week 4 : 계층형 질의 & 셀프 조인

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스 SQL 문제 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_4th_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**(수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 

## SQL_ADVANCED_4th

### 13.2.15 WITH (Common Table Expressions)

- **재귀 CTE를 통한 계층형 구조 탐색 방법을 중심으로 학습해주세요.**

### 13.2.10 SELECT Statement

- **동일 테이블 내에서 관계 탐색 하는 방법을 중심으로 학습해주세요**





### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**





> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - 13.2.15 WITH (Common Table Expressions) : MySQL 공식문서 
>
> https://dev.mysql.com/doc/refman/8.0/en/with.html
>
> (한국어 버전)
>
> - 13.2.10 SELECT Statement : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/select.html
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
| 4주차 | 계층형 질의와 셀프 조인 | ✅         |
| 5주차 | PIVOT / UNPIVOT         | 🍽️         |
| 6주차 | 정규 표현식             | 🍽️         |

<br>



## 문제 

- https://leetcode.com/problems/employees-earning-more-than-their-managers/ 

> LeetCode 181. Employees  Earning More Than Their Managers
>
> 학습 포인트 : 동일 테이블을 두 번 조인 (왜 동일 테이블을 JOIN 해야하는 문제일까)

- https://leetcode.com/problems/tree-node/description/

> LeetCode 608. Tree Node 
>
> 학습 포인트 : id, parent_id 기반의 트리 구조에서 **부모 ~ 자식 관계 재귀 탐색**
>
> Hint : (문제 해석) 
>
> - 어떤 노드가 Root Node 이려면, 부모노드가 존재하지 않아야 한다. 
> - 어떤 노드가 Inner Node 이려면, 나를 부모로 가지는 노드가 하나 이상 존재하여야 한다.
>   - 그 외네는 모두 Leaf Node 이다. --> (CASE 문을 사용하는 것을 추천드립니다.)

- https://school.programmers.co.kr/learn/courses/30/lessons/144856

> 프로그래머스 : 저자 별 카테고리 별 매출액 집계하기 
>
> 학습 포인트 : 카테고리와 서브카테고리 계층 구조를 분석하는 로직, SELF JOIN / CTE를 다 활용할 수 있다.
>
> - 위에 2가지의 문제를 풀어보고 난 이후, 더 편리한 방법으로 문제를 풀어보세요.



<!-- 여기까진 그대로 둬 주세요-->

---

 # 1. 계층형 질의 (WITH RECURSIVE)

~~~
✅ 학습 목표 :
* 'WITH RECURSIVE' 문법을 활용해 계층형 구조를 탐색할 수 있다.
~~~

✅ Recursive CTE
- CTE 안에서 자기 자신을 참조하는 CTE입니다.
  ```sql
  WITH RECURSIVE cte (n) AS
  (
    SELECT 1
    UNION ALL
    SELECT n + 1 FROM cte WHERE n < 5
  )
  SELECT * FROM cte;

+------+
| n    |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
|    5 |
+------+
  ```
- 구조
  ```sql
  SELECT ...      -- return initial row set
  UNION ALL
  SELECT ...      -- return additional row sets
  ```
- 비재귀 부분
  - 재귀적으로 생성될 값을 미리 고려해서 초기 SELECT에서 넉넉하게 길이를 확보하기 
    ```sql
    SELECT 1 AS n, CAST('abc' AS CHAR(20)) AS str
    ```
  - 컬럼이름대로 매칭하기
 
- 문법 제약
  - 집계 함수 (SUM(), AVG(), COUNT() 등)
  - 윈도우 함수
  - GROUP BY
  - ORDER BY
  - 초기값을 만드는 쿼리에서는 GROUP BY, DISTINCT, ORDER BY, LIMIT 모두 사용 가능!
  - CTE는 반드시 FROM 절에서 한 번만 참조해야 함

✅ 재귀 CTE에서 무한 루프를 방지하기 위한 종료 조건 및 제어 방법
- 재귀 CTE는 반드시 종료 조건이 있어야 함
- cte_max_recursion_depth 시스템 변수
  - 재귀 CTE가 최대 몇 번까지 반복(recursion)할 수 있는지를 제한하는 변수

✅ 예시

1. 피보나치 수열 (Fibonacci Series Generation)

```sql
WITH RECURSIVE fibonacci (n, fib_n, next_fib_n) AS (
  SELECT 1, 0, 1
  UNION ALL
  SELECT n + 1, next_fib_n, fib_n + next_fib_n
  FROM fibonacci
  WHERE n < 10
)
SELECT * FROM fibonacci;
```

2. 날짜 시퀀스 생성 (Date Series Generation)
```sql
WITH RECURSIVE dates (date) AS (
  SELECT MIN(date) FROM sales
  UNION ALL
  SELECT date + INTERVAL 1 DAY FROM dates
  WHERE date + INTERVAL 1 DAY <= (SELECT MAX(date) FROM sales)
)
SELECT * FROM dates;

3. 계층 구조 탐색 (Hierarchical Data Traversal)
- 비재귀 파트: CEO(manager_id IS NULL)를 시작점으로
- 재귀 파트: 상사의 ID가 나의 manager_id인 사람을 찾아 경로 연결
```sql
WITH RECURSIVE employee_paths (id, name, path) AS (
  SELECT id, name, CAST(id AS CHAR(200)) FROM employees
  WHERE manager_id IS NULL

  UNION ALL

  SELECT e.id, e.name, CONCAT(ep.path, ',', e.id)
  FROM employee_paths AS ep
  JOIN employees AS e ON ep.id = e.manager_id
)
SELECT * FROM employee_paths ORDER BY path;
```
✅ CTE의 장점 (Derived Table보다 더 나은 점)
1. CTE는 여러 번 참조 가능
2. CTE는 재귀(Self-referencing)가 가능
3. CTE끼리 참조 가능


# 2. 셀프 조인

~~~
✅ 학습 목표 :
* 같은 테이블 내에서 상호 관계를 탐색할 수 있는 셀프 조인의 구조를 이해하고 사용할 수 있다. 
~~~


<br>

<br>

---

# 확인문제

## 문제 1

> **🧚승화는 어떤 기업의 조직 구조를 분석하는 SQL 쿼리를 작성하고 있습니다. 각 직원은 상위 관리자 ID(manager_id)를 가지며, 조직도는 같은 Employees 테이블 내에서 계층적으로 연결됩니다. 승화는 최상위 관리자부터 각 사원까지의 계층 깊이(depth)를 계산하고자 다음과 같은 SELF JOIN 기반 쿼리를 시도했습니다.** 

~~~sql
SELECT e1.id, e1.name, e2.name AS manager_name
FROM Employees e1
LEFT JOIN Employees e2 ON e1.manager_id = e2.id;
~~~

> **쿼리를 잘 작성했다고 생각을 했지만, 막상 실행을 해보니 1단계 매니저까지만 추적할 수 있어 계층 구조의 전체를  표현하는데 한계가 존재했습니다. 이에 여러분에게 다음과 같은 미션을 요청합니다. WITH RECURSIVE를 활용하여  최상위 관리자부터 시작해 각 직원까지의 조직 구조 계층 깊이(depth)를 구하고, 결과를 depth가 높은 순으로 정렬하세요.**



~~~sql
WITH RECURSIVE employee_paths AS (
  SELECT id, name, manager_id, 1 AS depth -- 비재귀 영역 설정 (상위 관리자)
  FROM Employees
  WHERE manager_id IS NULL

  UNION ALL
  SELECT e.id, e.name, ep.name AS manager_name, ep.depth  + 1
  FROM employees e
  JOIN employee_paths ep ON e.manager_id = ep.id 
)
SELECT * FROM employee_path ORDER BY depth DESC;
~~~



<br>

### 🎉 수고하셨습니다.
