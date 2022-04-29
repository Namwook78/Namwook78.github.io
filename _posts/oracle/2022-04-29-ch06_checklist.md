---
title: "ch06_check_list"

categories:
  - "oracle"
tags:
  - [oracle]

toc: true
toc_sticky: true

date: 2022-04-29
last_modified_at: 2022-04-29
---

# 연습 문제

- - 4/29 미션 p.205 self check 6문제 풀기
-- p.256~257 문제풀기
- - 5시 30분 / 블로그 깃헙
-- 소스코드 설명 반드시 포함
- -
1. 101번 사원에 대해 아래의 결과를 산출하는 쿼리를 작성해 보자.

---

- 사번,  사원명,  job명칭,  job시작일자,  job종료일자,  job수행부서명
    
    <정답>
    

select

a.employee_id
, a.emp_name
, d.job_title
, b.start_date
, b.end_date
, c.department_name

from

employees a
, job_history b
, departments c
, jobs d

where

a.employee_id = b.employee_id
and b.department_id = c.department_id
and b.job_id = d.job_id
and a.employee_id = 101;

<해설>
-> 4개의 테이블, 즉 a,b,c,d 4개의 테이블들 중에 공통된 값을 가진 컬럼을 등호연산자로 연결해
4개의 컬럼 값들이 같은 행을 추출하여, a 테이블의 사원아이디가 101인 값들을 추출한다.

1. 아래의 쿼리를 수행하면 오류가 발생한다. 오류의 원인은 무엇인가?
    
    select
    
    a.employee_id
    , a.emp_name
    , b.job_id
    , b.department_id
    
    from
     employees a
    , job_history b
    
    where
    a.employee_id = b.employee_id(+)8
    and a.department_id(+) = b.department_id;
    

<해설>
-> 외부 조인의 경우, 조인조건에서 데이터가 없는 테이블의 컬럼에만 (+)를 붙여야 한다.
따라서 위 쿼리의 경우, and a.department_id(+) = b.department_id 가 아닌 and a.department_id = b.department_id(+)로 고쳐야 한다.

3 .외부조인시 (+)연산자를 같이 사용할 수 없는데, IN절에 사용하는 값이 1개인 경우는 사용 가능하다. 그 이유는 무엇일까?

<해설>
-> 오라클은 IN 절에 포함된 값을 기준으로 OR로 변환한다.
예를 들어,
employee_id IN (10, 20, 30)
= employee_id = 10 OR employee_id = 20 OR employee_id = 30 과 같은말이다 .

그런데 IN절에 값이 1개인 경우, 즉 employee_id IN (10)일 경우,
employee_id = 10 로 변환할 수 있으므로
외부조인을 하더라도 값이 1개인 경우는 사용할 수 있다.

1. 다음의 쿼리를 ANSI 문법으로 변경해 보자.
SELECT
a.department_id
, a.department_name

    
    FROM
    departments a
    , employees b
    
    WHERE
    a.department_id = b.department_id
    AND b.salary > 3000
    ORDER BY a.department_name;
    

<해설>
-> ANSI 내부조인은 FROM절에서 INNER JOIN 구문을 쓴다.
조인조건은 ON절에 명시하고, 조인 조건 외의 조건은 기존대로 WHERE 절에 명시한다. 만약 조인 조건
컬림이 두 테이블 모두 동일하다면 ON 대신 USING 절을 사용할 수 있는데, 이때는 SELECT절에서 조인
조건에 포함된 컬럼명을 테이블명,컬럼명형태가 아닌 컬럼명만 기술해야 한다.

SELECT
  a.department_id
, a.department_name

FROM
   departments a
   INNER JOIN -
   employees b
   ON ( a.department_id = b.department_id )

WHERE

 b.salary > 3000
ORDER BY a.department_name;

1. 다음은 연관성 있는 서브쿼리이다. 이를 연관성 없는 서브쿼리로 변환해 보자.

    
    SELECT
    a.department_id
    , a.department_name
    
    FROM
    departments a
    
    WHERE EXISTS 
    
    ( SELECT 1
    FROM job_history b
    WHERE a.department_id = b.department_id );
    

<해설> -> WHERE EXISTS 구문에서 EXISTS 뺸후, IN절안에 WHERE 구문을 빼면 연관성이 없어진다.
SELECT
a.department_id
, a.department_name
FROM
departments a
WHERE a.department_id IN ( SELECT department_id
FROM job_history  );

1. 연도별 이태리 최대매출액과 사원을 작성하는 쿼리를 학습했다. 이 쿼리를 기준으로 최대 매출액 뿐만 아니라 최소매출액과 해당 사원을 조회하는 쿼리를 작성해 보자.

    
    <해설>
    -> 연도, 사원별 이탈리아 매출액 구하기
    -> 위에서 구한 결과에서 연도별 최대, 최소 매출액 구하기
    -> 위2개의 결과를 조인해서 최대매출, 최소매출액을 일으킨 사원을 찾아야 하므로, 위2개의
    결과를 인라인 뷰로 만든다
    -> 마지막으로 인라인 뷰의 결과와 사원 테이블을 조인해서 이름을 가져온다
    
    SELECT emp.years,
    emp.employee_id,
    emp2.emp_name,
    emp.amount_sold
    
    FROM
    ( SELECT SUBSTR(a.sales_month, 1, 4) as years,
    a.employee_id,
    SUM(a.amount_sold) AS amount_sold
    
    FROM
    sales a
    , customers b
    , countries c
    
    WHERE
    a.cust_id = b.CUST_ID
    AND b.country_id = c.COUNTRY_ID
    AND c.country_name = 'Italy'
    GROUP BY SUBSTR\
    (a.sales_month, 1, 4), a.employee_id
    ) emp,
    ( SELECT years,
    MAX(amount_sold) AS max_sold,
    MIN(amount_sold) AS min_sold
    
    FROM
    ( SELECT SUBSTR(a.sales_month, 1, 4) as years,
    a.employee_id,
    SUM(a.amount_sold) AS amount_sold
    
    FROM
    sales a
    , customers b
    , countries c
    
    WHERE
    a.cust_id = b.CUST_ID
    AND b.country_id = c.COUNTRY_ID
    AND c.country_name = 'Italy'
    GROUP BY SUBSTR
    (a.sales_month, 1, 4), a.employee_id
    ) K
    GROUP BY years
    ) sale,
    employees emp2
    
    WHERE
    emp.years = sale.years
    AND (emp.amount_sold = sale.max_sold OR emp.amount_sold = sale.min_sold)
    AND emp.employee_id = emp2.employee_id
    
    ORDER BY years;