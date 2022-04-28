---
title: "oracle 설치 및 db 작성법"

categories:
  - "oracle"
tags:
  - "oracle"

toc: true
toc_sticky: true

date: 2022-04-28
last_modified_at: 2022-04-28
---

# Oracel 설치 및 DB작성법

맨마지막 setup 관리자 권한 실행 -> 단일 인스턴스 데이터 베이스 생성 및 구성
->데스크톱 클래스 -> 가상 계정 사용 -> 전역 데이터베이스 이름: myoralce, 문자집합 유니코드, 비밀번호 1234 , 유니코드: 컨테이너 데이터베이스로 생성 체크해제
->다음 ->

ERROR 시 -> 소프트웨어 생성 -> 가상 계정 사용 -> 전역 데이터베이스 없음 ,DB 생성
->cmd명령프롬프트 경로설정 C:\sql_lecture\WINDOWS.X64_193000_db_home>dbca -> 단일  인스턴트 -> 가상계좌생성 -> myoracle,
1234,1234,컨테이너 데이터베이스로 생성 체크해제
dbca

<SQL -plus>
사용자명 입력: system
비밀번호 입력: 1234
SQL>CREATE TABLESPACE myts DATAFILE 'C:\sql_lecture\oradata\MYORACLE\myts.dbf' SIZE 100M AUTOEXTEND ON NEXT 5M;
CREATE USER ora_user IDENTIFIED BY evan DEFAULT TABLESPACE MYTS TEMPORARY TABLESPACE TEMP;
(사용자이름:ora_user) -아이디 바꾸는게 좋음
(비밀번호 evan) -> 비밀번호 바꾸는게 좋음
GRANT DBA TO ora_user;
connect ora_user/evan;
select user from dual;

sql developer압축풀기 -> sql developer실행
새 데이터베이스 접속 -> name:ora_user ->사용자이름:ora_user -> 비밀반호 evan ->SID:myoralce - > 테스트 -> 접속
#sql developer the network adapter could not establish the connection 오류 ->  Net configuration Assistant -> 리눅스 생성 -> 계속다음
-> cmd -> C:\sql_lecture>lsnrctl status -> sql_developer에서 새 데이터 베이스 생성 (위에 거와 같음) name~~접속

강사님 github에서 expall,expcust 다운로드후 C드라이브 back_up파일에 넣기 (back_up)파일 새로 만들고 C드라이브에 넣어야함.

(경로)C:\back_up>imp ora_user/evan file=expall.dmp log=empall.log ignore=y grants=y rows=y indexes=y full=y
imp ora_user/evan file=expcust.dmp log=expcust.log ignore=y grants=y rows=y indexes=y full=y
SELECT table_name FROM user_tables;(sql_developer)

SQL developer 새데이터 베이스 -> SELECT table_name FROM user_tables; -> F5 -> 10개행 실행

SQL plus system evan connect ora_user; show user; -> "ORA_USER"

SQL Develpoer (x), VS code | pycharm | 이클립스 연동 -> 개발자는 이클립스 연동으로 주로 작업

CREATE TABLE ex2_1 (
COLUMN1     CHAR(10)
, COLUMN2     VARCHAR2(10)
, COLUMN3     NVARCHAR2(10)
, COLUMN4     NUMBER
); -> 교재보다 이렇게 쓰는게 작업할때 더 편리하다

- - P.49 테이블 생성
-- 쿼리를 작성 한 후, F5 실행
-- SQL Develpoer (x), VS code | pycharm | 이클립스 연동
CREATE TABLE ex2_1 (
COLUMN1 CHAR(10)
, COLUMN2 VARCHAR2(10)
, COLUMN3 NVARCHAR2(10)
, COLUMN4 NUMBER
);
-- 데이터 추가
INSERT INTO ex2_1 (column1, column2) VALUES('abc', 'abc');
- - 데이터 확인
SELECT
COLUMN1
, LENGTH(COLUMN1) as len1
, COLUMN2
, LENGTH(COLUMN1) as len2
FROM ex2_1;

CREATE TABLE ex2_2(
COLUMN1    VARCHAR2(3)
, COLUMN2     VARCHAR2(3 byte)
, COLUMN3     VARCHAR2(3 char)
);

INSERT INTO ex2_2 VALUES('abc', 'abc', 'abc');

SELECT column1, LENGTH(column1) AS len1,
column2, LENGTH(column2) AS len2,
column3, LENGTH(column3) AS len3
FROM ex2_2;

- - 한글 데이터 삽입
INSERT INTO ex2_2 VALUES('홍길동', '홍길동', '홍길동');
- - Column3에만 데이터 추가
INSERT INTO ex2_2 (column3) VALUES('홍길동');
- - 데이터 조회
SELECT
COLUMN3
, LENGTH(COLUMN3) AS len3
, LENGTHB(COlUMN3) AS bytelen
FROM ex2_2;
- - 숫자 데이터 삽입
CREATE TABLE ex2_3(
COL_INT INTEGER
, COL_DEC DECIMAL
, COL_NUM NUMBER
);

SELECT
column_id
, column_name
, data_type
, data_length
FROM user_tab_cols
WHERE table_name = 'EX2_3'
ORDER BY column_id;

- - p57(FLOAT형으로 인한 오류)
CREATE TABLE ex2_4 (
COL_FLOT1 FLOAT(32),
COL_FLOT2 FLOAT
);

INSERT INTO ex2_4 (col_flot1, col_flot2) VALUES (1234567891234, 1234567891234);

- - 조회
SELECT * FROM ex2_4;
- - p58 (날짜 데이터 타입)
CREATE TABLE ex2_5(
COL_DATE DATE
, COL_TIMESTAMP TIMESTAMP
);

INSERT INTO ex2_5 VALUES (SYSDATE, SYSTIMESTAMP);

SELECT * FROM ex2_5;

- - p59
-- LOB 데이터 타입
-- Large Object의 약자로 대용량 데이터 저장할 수 있는 데이터 타입
-- 비정형 데이터는 그 크기가 매우 큰데, 이런 데이터를 저장한다.

- - P.66 CHECK
CREATE TABLE ex2_9 (
num1 NUMBER
CONSTRAINTS check1 CHECK ( num1 BETWEEN 1 AND 9),
gender VARCHAR2(10)
CONSTRAINTS check2 CHECK ( gender IN ('MALE', 'FEMALE'))
);

SELECT constraint_name, constraint_type, table_name, search_condition
FROM user_constraints
WHERE table_name = 'EX2_9';

INSERT INTO ex2_9 VALUES (10, 'MAN');

INSERT INTO ex2_9 VALUES (5, 'FEMALE');

- - DEFAULT
-- PL/SQL

CREATE TABLE ex2_10 (
Col1        VARCHAR2(10) NOT NULL,
Col2        VARCHAR2(10) NULL,
Create_date DATE DEFAULT SYSTIMESTAMP);

- - COL1 COL2 사용자가 입력
-- Create_Date DB에서 자동으로 입력
INSERT INTO ex2_10(col1, col2) VALUES ('AA', 'BB');

SELECT * FROM ex2_10;

- - 테이블 변경
--P.69 (1).컬럼명 변경
ALTER TABLE ex2_10 RENAME COLUMN Col1 TO COL11;
SELECT * FROM ex2_10;
- - 컬럼 내역 확인법
DESC ex2_10;
- - (2) 컬럼 타입 변경
-- 컬럼 타입 변경 (VARCHAR2 (10) ~ VARCHAR2 (30))으로 변경
ALTER TABLE ex2_10 MODIFY Col2 VARCHAR2(30);
DESC ex2_10;
- - (3) 새로운 컬럼 추가
ALTER TABLE ex2_10 ADD Col3 NUMBER;
DESC ex2_10;
- - (4) 컬럼 삭제
ALTER TABLE ex2_10 DROP COLUMN Col3;
DESC ex2_10;
- - 제약조건 추가
ALTER TABLE ex2_10 ADD CONSTRAINTS pk_ex2_10 PRIMARY KEY(col11);
- - USER CONSTARAINTS 제약 조건 확인
SELECT constraint_name, constraint_type, table_name, search_condition
FROM user_constraints
WHERE table_name = 'EX2_10';
- -제약조건 삭제
ALTER TABLE ex2_10 DROP CONSTRAINTS pk_ex2_10;
SELECT constraint_name, constraint_type, table_name, search_condition
FROM user_constraints
WHERE table_name = 'EX2_10';
- - 테이블 복사
-- P.72
CREATE TABLE eX2_9_1 AS SELECT * FROM ex2_9;
- - 뷰(view)
-- 테이블이랑 비슷
SELECT * FROM employees;
SELECT * FROM departments;
- - 2개의 테이블 matching 후 하나의 테이블로 생성
CREATE OR REPLACE VIEW emp_dept_vl AS
SELECT
a.employee_id
, a.emp_name
, a.department_id -- 부서명 컬럼
, b.department_name
FROM
employees a
, departments b
WHERE a.department_id = b.department_id -- 조건식

SELECT * FROM emp_dept_vl

- - 인덱스 생성
-- **추후 공부해야 할 내용 인덱스 내부 구조에 따른 분류**
---- (초중급 레벨) B-Tree 인덱스, 비트맵 인덱스, 함수 기반 인덱스
---- DB 성능
-- 인덱스 생성
-- col11 값에 중복 값을 허용하지 않는다.
-- 인덱스 생성 시, user_indexes 시스템 뷰에서 내역 확인
CREATE UNIQUE INDEX ex2_10_ix01
ON ex2_10 (col11);