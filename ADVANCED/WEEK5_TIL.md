# SQL_ADVANCED 5주차 정규 과제

## **Week 5 : PIVOT / UNPIVOT**

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스 SQL 문제 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다.

이번 주는 아래의 **SQL_ADVANCED_5th_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 ‘SQL’ 시트에 링크를 제출**해주세요.

**(수행 인증샷은 필수입니다.)**

> 프로그래머스 문제를 풀고 ‘정답입니다’ 문구를 캡쳐해서 올려주시면 됩니다.

------

## **SQL_ADVANCED_5th_TIL**

- MySQL 공식문서에는 `PIVOT / UNPIVOT`에 대한 **전용 문법을 제공하지 않고 있습니다.** 따라서 이번 주차에서는 PIVOT / UNPIVOT에 대한 개념을 이해하고, 이를 `CASE WHEN + GROUP BY` 또는 `UNION ALL` 등의 조합으로 수동 구현할 수 있는 방법을 학습하면 됩니다. 

참고할 자료 

1. **PIVOT / UNPIVOT 개념 학습 Blog**

https://m.blog.naver.com/regenesis90/222205833002

https://blog.naver.com/regenesis90/222205964866

2. **MySQL 로 PIVOT 구현하기**

https://shxrecord.tistory.com/181





## **🏁 강의 수강 (Study Schedule)**

| **주차** | **공부 범위**           | **완료 여부** |
| -------- | ----------------------- | ------------- |
| 0주차    | 서브쿼리 & CTE          | ✅             |
| 1주차    | 집합 연산자 & 그룹 함수 | ✅             |
| 2주차    | 윈도우 함수             | ✅             |
| 3주차    | Top N 쿼리              | ✅             |
| 4주차    | 계층형 질의 & 셀프 조인 | ✅             |
| 5주차    | PIVOT / UNPIVOT         | ✅             |
| 6주차    | 정규 표현식             | 🍽️             |



## 문제

https://leetcode.com/problems/reformat-department-table/description/

> LeetCode. Reformat Department Table
>
> 학습 포인트 : MySQL 에서는 PIVOT을 쉽게 구할 수 있는 방법이 없다. 
> <img width="1434" height="558" alt="image" src="https://github.com/user-attachments/assets/ea343cd4-7cb7-4dfa-8668-e1677d3311ec" />
> - 수동으로 구하기 : CASE WHEN + 집계함수 / GROUP BY + 조건 분기 사용



---

## **1. PIVOT**

```
✅ 학습 목표 :
* MySQL에서 직접적인 `PIVOT` 문법은 없으므로, `CASE WHEN`, `GROUP BY`, `MAX()` 등으로 대체 구현할 수 있다.
* 데이터를 행 → 열 방향으로 전개하는 기본 로직을 이해한다.
```

1. PIVOT의 이해와 표현
1) PIVOT 이해
✅ 피벗 테이블(pivot table): 표의 행과 열을 전환하는 등의 과정을 통하여 통계를 재정렬하고, 그 결과로 표 데이터를 요약하는 방법. 이에는 집계함수(합계, 평균 등)가 사용될 수 있습니다. 
- 2가지 컬럼(X, Y)에 따라 그룹화되어 1개의 컬럼으로 표현된 집계함수 정보(K)를
- X, Y 중 하나를 행렬전환하여, K값을 행과 열의 2차원적 정보로 조회할 수 있게 한다.

2) 오라클SQL에서 PIVOT 표현

- 적용 전
​```sql
select 컬럼이름X,
       컬럼이름Y,
       집계함수(컬럼이름K) as 별명K
from 테이블이름A
group by 컬럼이름X,
         컬럼이름Y;
```

- 적용 후
```sql
with temp as
    (select 컬럼이름X,
            컬럼이름Y,
            컬럼이름K
     from 테이블이름A)
select * from temp
pivot (
    집계함수(컬럼이름K) as 별명K
    for 컬럼이름Y in (컬럼X값1 as 별명X1,
                     컬럼X값2 as 별명X2,
                     ...)
);
```

2. 예제 : PIVOT 미사용 / 사용 결과 비교
1) 예제 : PIVOT을 사용하지 않는 경우(UNPIVOT) employees 테이블에서 직책(job_id) 및 부서(department_id) 별 직원 수를 출력하기 
```sql
SELECT
  job_id,
  department_id,
  count(employee_id) as count
FROM Table
GROUP BY
  job_id,
  department_id;
```
2) 예제 : PIVOT을 사용 employees 테이블에서 직책(job_id) 및 부서(department_id) 별 직원 수를 출력하기 
```sql
with temp as
    (select job_id,
            department_id,
            employee_id
     from employees)
select * from temp
pivot (
    count(employee_id) as c
    for department_id in (10 as d10,
                          20 as d20,
                          30 as d30,
                          40 as d40,
                          50 as d50,
                          60 as d60,
                          70 as d70,
                          80 as d80,
                          90 as d90,
                          100 as d100,
                          null as none)
);
```
* 부서 변수가 컬럼이 아닌, 가로 행렬로 표현됨


## **2. UNPIVOT**

```
✅ 학습 목표 :
* UNPIVOT 역시 직접 문법이 없으므로, `UNION ALL`과 `JOIN`, JSON 등의 방법으로 열 → 행 형태로 변환하는 과정을 익힌다.
* `UNION ALL`로 수동 구현 시의 컬럼 이름 통일과 데이터 병합 과정을 익힌다.
```

1. UNPIVOT의 이해와 표현
1) UNPIVOT의 이해
UNPIVOT은 PIVOT의 반대 역할을 수행합니다. 즉, 피벗 테이블을 해제하거나, 피벗 테이블 형식의 테이블을 그렇지 않은 형태로 만듭니다. 

2) 기본식 : UNPIVOT의 표현
```sql
select * from table1
UNPIVOT (

select * from 테이블이름A
UNPIVOT (
컬럼이름K for 컬럼이름Y in (컬럼이름Y1_K as 컬럼Y값1,
                           컬럼이름Y2_K as 컬럼Y값2,
                           ...)
);
```
2. 예제 : UNPIVOT 사용
```sql
select * from pivot_sample
UNPIVOT (
count_employee for department_id in (d10_c as 10,
                                     d20_c as 20,
                                     d30_c as 30,
                                     d40_c as 40,
                                     d50_c as 50,
                                     d60_c as 60,
                                     d70_c as 70,
                                     d80_c as 80,
                                     d90_c as 90,
                                     d100_c as 100,
                                     none_c as null)
);
```

[MySQL,MariaDB]피벗(Pivot) 사용하기
```sql
SELECT DISTINCT 	 
       B.USERCD
	  , B.WORKDAY
	  , A.DATECHAR
	  , B.TODATE
	  , B.FROMDATE
 FROM PM_CALENDAR A
 	   LEFT OUTER JOIN PM_COMMUTE B
 	   ON A.DATECHAR = B.WORKDAY
      AND B.CMPCD = 'P0001'
WHERE A.DATECHAR BETWEEN '20200420' AND '20200430'
ORDER BY A.DATECHAR
```

1) 피벗 1 - PM_CALENDAR
먼저, 날짜 데이터가 있는 PM_CALENDAR 데이터를 피벗.

```sql
SELECT 
      '' AS USERCD			
      , MAX(IF(B.ROWNUM = '20', B.WORKDAY, 0)) AS WORKDAY20
      , MAX(IF(B.ROWNUM = '21', B.WORKDAY, 0)) AS WORKDAY21
      , MAX(IF(B.ROWNUM = '22', B.WORKDAY, 0)) AS WORKDAY22
      , MAX(IF(B.ROWNUM = '23', B.WORKDAY, 0)) AS WORKDAY23
      , MAX(IF(B.ROWNUM = '24', B.WORKDAY, 0)) AS WORKDAY24
      , MAX(IF(B.ROWNUM = '25', B.WORKDAY, 0)) AS WORKDAY25
      , MAX(IF(B.ROWNUM = '26', B.WORKDAY, 0)) AS WORKDAY26
      , MAX(IF(B.ROWNUM = '27', B.WORKDAY, 0)) AS WORKDAY27
      , MAX(IF(B.ROWNUM = '28', B.WORKDAY, 0)) AS WORKDAY28				
      , MAX(IF(B.ROWNUM = '29', B.WORKDAY, 0)) AS WORKDAY29			
      , MAX(IF(B.ROWNUM = '30', B.WORKDAY, 0)) AS WORKDAY30		
      , MAX(IF(B.ROWNUM = '31', B.WORKDAY, 0)) AS WORKDAY31		
FROM (
    SELECT
      CONCAT(SUBSTRING(A.DATECHAR, 5, 2), '.', SUBSTRING(A.DATECHAR, 7, 2)) WORKDAY, SUBSTRING(A.DATECHAR,7,2) AS ROWNUM
    FROM PM_CALENDER A
		WHERE A.DATECHAR BETWEEN '20200420' AND '20200430'
	  ) B
```

2) 피벗 2 - PM_COMMUTE
```sql
SELECT 		
  	  A.USERCD			
	, MAX(IF(ROWNUM = '20', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY20
	, MAX(IF(ROWNUM = '21', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY21
	, MAX(IF(ROWNUM = '22', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY22
	, MAX(IF(ROWNUM = '23', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY23
	, MAX(IF(ROWNUM = '24', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY24
	, MAX(IF(ROWNUM = '25', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY25
	, MAX(IF(ROWNUM = '26', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY26
	, MAX(IF(ROWNUM = '27', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY27
	, MAX(IF(ROWNUM = '28', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY28
	, MAX(IF(ROWNUM = '29', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY29
	, MAX(IF(ROWNUM = '30', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY30
	, MAX(IF(ROWNUM = '31', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY31	
 FROM (
		 SELECT   	 
		       USERCD
			  , SUBSTRING(WORKDAY,7,2) AS ROWNUM
			  , IFNULL(DATE_FORMAT(TODATE, '%H:%i'),'') TODATE		
			  , IFNULL(DATE_FORMAT(FROMDATE, '%H:%i'),'') FROMDATE
		 FROM PM_COMMUTE
		WHERE WORKDAY BETWEEN '20200420' AND '20200430'
 		  AND CMPCD = 'P0001'
 	  ) A
GROUP BY A.USERCD	
```
3) UNION ALL
```sql
SELECT 
			  '' AS USERCD			
			, MAX(IF(B.ROWNUM = '20', B.WORKDAY, 0)) AS WORKDAY20
			, MAX(IF(B.ROWNUM = '21', B.WORKDAY, 0)) AS WORKDAY21
			, MAX(IF(B.ROWNUM = '22', B.WORKDAY, 0)) AS WORKDAY22
			, MAX(IF(B.ROWNUM = '23', B.WORKDAY, 0)) AS WORKDAY23
			, MAX(IF(B.ROWNUM = '24', B.WORKDAY, 0)) AS WORKDAY24
			, MAX(IF(B.ROWNUM = '25', B.WORKDAY, 0)) AS WORKDAY25
			, MAX(IF(B.ROWNUM = '26', B.WORKDAY, 0)) AS WORKDAY26
			, MAX(IF(B.ROWNUM = '27', B.WORKDAY, 0)) AS WORKDAY27
			, MAX(IF(B.ROWNUM = '28', B.WORKDAY, 0)) AS WORKDAY28				
			, MAX(IF(B.ROWNUM = '29', B.WORKDAY, 0)) AS WORKDAY29			
			, MAX(IF(B.ROWNUM = '30', B.WORKDAY, 0)) AS WORKDAY30		
			, MAX(IF(B.ROWNUM = '31', B.WORKDAY, 0)) AS WORKDAY31		
		FROM (
				SELECT 
					CONCAT(SUBSTRING(A.DATECHAR,5,2),'.',SUBSTRING(A.DATECHAR,7,2)) WORKDAY
			    , SUBSTRING(A.DATECHAR,7,2) AS ROWNUM
				FROM PM_CALENDAR A
				WHERE A.DATECHAR BETWEEN '20200420' AND '20200430'
			  ) B
		UNION ALL
		SELECT 		
		  	  A.USERCD			
			, MAX(IF(ROWNUM = '20', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY20
			, MAX(IF(ROWNUM = '21', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY21
			, MAX(IF(ROWNUM = '22', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY22
			, MAX(IF(ROWNUM = '23', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY23
			, MAX(IF(ROWNUM = '24', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY24
			, MAX(IF(ROWNUM = '25', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY25
			, MAX(IF(ROWNUM = '26', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY26
			, MAX(IF(ROWNUM = '27', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY27
			, MAX(IF(ROWNUM = '28', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY28
			, MAX(IF(ROWNUM = '29', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY29
			, MAX(IF(ROWNUM = '30', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY30
			, MAX(IF(ROWNUM = '31', CONCAT(A.TODATE, '<br>/', A.FROMDATE), '-')) AS WORKDAY31					
		 FROM (
				 SELECT   	 
				       USERCD
					  , SUBSTRING(WORKDAY,7,2) AS ROWNUM
					  , IFNULL(DATE_FORMAT(TODATE, '%H:%i'),'') TODATE		
					  , IFNULL(DATE_FORMAT(FROMDATE, '%H:%i'),'') FROMDATE
				 FROM PM_COMMUTE
				WHERE WORKDAY BETWEEN '20200420' AND '20200430'
		 		  AND CMPCD = 'P0001'
		 	  ) A
GROUP BY A.USERCD		 	 
```


## **확인문제**

### **문제 1**

> **🧚지민이는 매월 각 매장의 월별 매출 데이터가 담긴 테이블을 가공하려 합니다. 아래와 같은 테이블이 있다고 가정합시다.**

| **branch** | **Jan_sales** | **Feb_sales** | **Mar_sales** |
| ---------- | ------------- | ------------- | ------------- |
| A          | 100           | 120           | 130           |
| B          | 90            | 110           | 140           |

> **Q. 이 테이블을 아래와 같은 형태로 바꾸고 싶습니다. SQL에서 UNION ALL을 활용하여 UNPIVOT 구조를 수동으로 구현해보세요.**

| **branch** | **month** | **sales** |
| ---------- | --------- | --------- |
| A          | Jan       | 100       |
| A          | Feb       | 120       |
| A          | Mar       | 130       |
| B          | Jan       | 90        |
| B          | Feb       | 110       |
| B          | Mar       | 140       |

```sql
SELECT branch, 'Jan' AS month, Jan_sales AS sales
FROM sales_table
UNION ALL
SELECT branch, 'Feb' AS month, Feb_sales AS sales
FROM sales_table
UNION ALL
SELECT branch, 'Mar' AS month, Mar_sales AS sales
FROM sales_table;
```

### 문제 2

> **🧚예린이는 지점별로 월별 매출을 한 눈에 보기 위해, 아래와 같이 매출 데이터가 저장된 데이터를 가공하려고 합니다.**

| **branch** | **month** | **sales** |
| ---------- | --------- | --------- |
| A          | Jan       | 100       |
| A          | Feb       | 120       |
| A          | Mar       | 130       |
| B          | Jan       | 90        |
| B          | Feb       | 110       |
| B          | Mar       | 140       |

> **이 데이터를 아래와 같이 월별 매출 컬럼이 각각 존재하도록 PIVOT 형태로 바꾸고 싶습니다.MySQL에서는 PIVOT 문법이 없기 때문에, CASE WHEN, GROUP BY, MAX() 또는 SUM() 등을 이용해 수동으로 구현해보세요.**

- 원하는 결과 

| **branch** | **Jan_sales** | **Feb_sales** | **Mar_sales** |
| ---------- | ------------- | ------------- | ------------- |
| A          | 100           | 120           | 130           |
| B          | 90            | 110           | 140           |



~~~sql
SELECT
  branch,
  SUM(CASE WHEN month = 'Jan' THEN sales ELSE 0 END) AS Jan_sales,
  SUM(CASE WHEN month = 'Feb' THEN sales ELSE 0 END) AS Feb_sales,
  SUM(CASE WHEN month = 'Mar' THEN sales ELSE 0 END) AS Mar_sales
FROM sales_table
GROUP BY BRANCH 
~~~






### **🎉 수고하셨습니다.**
