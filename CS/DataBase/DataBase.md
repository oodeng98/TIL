# DataBase
체계적인 데이터 모음  

데이터
- 저장이나 처리에 효율적인 형태로 변환된 정보

기존 데이터 저장 방식  
1. File 이용
- 어디에서나 쉽게 사용 가능
- 데이터를 구조적으로 관리하기 어려움
2. Spreadsheet 이용
- 테이블의 열과 행을 사용해 데이터를 구조적으로 관리 가능
- 스프레드 시트의 한계
  - 크기: 일반적으로 약 100만 행까지만 저장 가능
  - 보안: 단순히 파일이나 링크 소유 여부에 따른 단순한 접근 권한 기능 제공
  - 정확성
    - 만약 Spreadsheet의 내용을 갱신해야 한다면?
    - 데이터가 여러 시트에 분산되어 있다면 변경에 누락이 생기거나 문제가 발생할 수 있음  


## Relational Database
데이터베이스의 역할
- 데이터를 저장(구조적 저장)하고 조작(CRUD)  

Relational Database(관계형 데이터베이스)
- 데이터 관에 관계가 있는 데이터 항목들의 모음
- 테이블, 행, 열의 정보를 구조화하는 방식
- 서로 관련된 데이터 포인터를 저장하고 이에 대한 엑세스를 제공  

Relation(관계)
- 여러 테이블 간의 논리적 연결
- 아래 그림처럼 관계를 통해 두 테이블을 사용하여 데이터를 다양한 형식으로 조회할 수 있음
  - ex)특정 날짜에 구매한 모든 고객 조회, 지난 달에 배송일이 지연된 고객 조회 등

![relation](./img/relation.PNG)

관계형 데이터베이스 관련 키워드
1. Table(Relation)
- 데이터를 기록하는 곳
2. Field(Column, Attribute)
- 각 필드에는 고유한 데이터 타입이 지정됨
3. Record(Row, Tuple)
- 각 레코드에는 데이터 값이 저장됨
4. Database(Schema)
- 테이블의 집합
5. Primary Key(PK)
- 각 레코드의 고유한 값
- 관계형 데이터베이스에서 레코드의 식별자로 활용
6. Foreign Key(FK, 외래 키)
- 테이블의 필드 중 다른 테이블의 레코드를 식별할 수 있는 키
- 다른 테이블의 기본 키를 참조
- 각 레코드에서 서로 다른 테이블 간의 관계를 만드는 데 사용

### RDBMS
DBMS(Database Management System)
- 데이터베이스를 관리하는 소프트웨어 프로그램
- 데이터 저장 및 관리를 용이하게 하는 시스템
- 데이터베이스와 사용자 간의 인터페이스 역할
- 사용자가 데이터 구성, 업데이트, 모니터링, 백업, 복구 등을 할 수 있도록 도움  

RDBMS(Relational DBMS)
- 관계형 데이터베이스를 관리하는 소프트웨어 프로그램

RDBMS 서비스 종류
- SQLite
- MySQL
- PostgreSQL
- Oracle Database
- ...

## SQL
Structure Query Language
- 데이터베이스에 정보를 저장하고 처리하기 위한 프로그래밍 언어
- 테이블의 형태로 구조화(Structure)된 관계형 데이터베이스에게 요청을 질의(Query)

### SQL Syntax
1. SQL 키워드는 대소문자를 구분하지 않음
- 하지만 대문자로 작성하는 것을 권장(명시적 구분)

2. 각 SQL Statements의 끝에는 ';'이 필요
- 세미콜론은 각 SQL Statements를 구분하는 방법(명령어의 마침표)

### SQL Statements
SQL을 구성하는 가장 기본적인 코드 블록  

```SQL
SELECT column_name FROM table_name;
```
위의 코드는 SELECT Statement라고 부름  
이 Statement는 SELECT, FROM 2개의 keyword로 구성됨  

수행 목적에 따른 SQL Statements 유형
|유형|역할|SQL keyword|
|---|---|------------|
|DDL(Definition)|데이터의 기본 구조 및 형식 변경|CREATE, DROP, ALTER|
|DQL(Query)|데이터 검색|SELECT|
|DML()|데이터 조작(추가, 수정, 삭제)|INSERT, UPDATE, DELETE|
|DCL|데이터 및 작업에 대한 사용자 권한 제어|COMMIT, ROLLBACK, GRANK, REVOKE|

## Single Table Queries

### Querying data
SELECT statement
- 테이블에서 데이터를 조회

SELECT syntax  
```sql
SELECT select_list
-- SELECT 키워드 이후 데이터를 선택하려는 필드를 하나 이상 지정
FROM table_name;
-- FROM 키워드 이후 데이터를 선택하려는 테이블의 이름을 지정
```

SELECT Query 예시
1. 테이블 employees에서 LastName, FirstName 필드의 모든 데이터를 조회
```sql
SELECT LastName, FirstName
FROM employees;
```
2. 테이블 employees에서 모든 필드 데이터를 조회
```sql
SELECT *
FROM employees;
```

3. 테이블 employees에서 FirstName 필드의 모든 데이터를 조회, 조회 시 FirstName이 아닌 '이름'으로 출력될 수 있도록 변경
```sql
SELECT FirstName AS '이름'
FROM employees;
```

3. 테이블 tracks에서 Name, Milliseconds 필드의 모든 데이터를 조회, 조회 시 Milliseconds 필드는 60000으로 나눠 분 단위 값으로 출력하고 조회 시 '재생 시간(분)'으로 출력될 수 있도록 변경
```sql
SELECT Name, Milliseconds / 60000 AS '재생 시간(분)'
FROM tracks;
```

### Sorting data
ORDER BY statement
- 조회 결과의 레코드를 정렬  

ORDER BY syntax
- FROM clause 뒤에 위치
- 하나 이상의 column을 기준으로 결과를 오름차순(ASC, 기본 값), 내림차순(DESC)으로 정렬

ORDER BY Query 예시  
1. 테이블 employees에서 FirstName 필드의 모든 데이터를 오름차순으로 조회
```sql
SELECT FirstName
FROM employees
ORDER BY FirstName;
ORDER BY FirstName DESC; -- 내림차순 조회
```

2. 테이블 customers에서 Country 필드를 기준으로 내림차순 정렬한 다음 City 필드 기준으로 오름차순 정렬하여 조회
```sql
SELECT Country, City
FROM customers
ORDER BY Country DESC, City;
```

정렬에서의 NULL
- NULL 값이 존재할 경우 오름차순 정렬 시 결과에 NULL이 먼저 출력  

SELECT statement 실행 순서  
1. 테이블에서(FROM)
2. 조회하여(SELECT)
3. 정렬(ORDER BY)

### Filtering data
Filtering data keywords
- Clause
  - DISTINCT
  - WHERE
  - LIMIT
- OPERATOR
  - BETWEEN
  - IN
  - LIKE
  - Comparison
  - Logical

#### DISTINCT statement
조회 결과에서 중복된 레코드를 제거  

DISTINCT syntax
- SELECT 키워드 바로 뒤에 작성해야 함
- SELECT DISTINCT 키워드 다음에 고유한 값을 선택하려는 하나 이상의 필드를 지정

DISTINCT Query 예시
1. 테이블 customers에서 Country 필드의 모든 데이터를 중복없이 오름차순 조회
```sql
SELECT DISTINCT Country
FROM customers
ORDER BY Country;
```

#### WHERE statement
조회 시 특정 검색 조건을 지정  

WHERE syntax
- FROM clause 뒤에 위치
- search_condition은 비교연산자 및 논리연산자(AND, OR, NOT 등)를 사용하는 구문이 사용됨

WHERE Query 예시  
1. 테이블 customers에서 City 필드 값이 'Prague'인 데이터의 LastName, FirstName, City 조회
```sql
SELECT LastName, FirstName, City
FROM customers
WHERE City = 'Prague';
-- WHERE City != 'Prague';  -- Prague가 아닌 값
```
2. 테이블 customers에서 Company 필드 값이 NULL이고 Country 필드 값이 'USA'인 데이터의 LastName, FirstName, Company, Country 조회
```sql
SELECT LastName, FirstName, Company, Country
FROM customers
WHERE Company IS NULL AND Country = 'USA'; -- 물론 OR도 사용 가능
```
3. 테이블 tracks에서 Bytes 필드 값이 100000 이상 500000 이하인 데이터의 Name, Bytes 조회
```sql
SELECT Name, Bytes
FROM tracks
WHERE Bytes BETWEEN 100000 and 500000;
```
4. 테이블 customers에서 Country 필드 값이 'Canada', 'Germany', 혹은 'France'인 데이터의 LastName, FirstName, Country 조회
```sql
SELECT LastName, FirstName, Country
FROM customers
WHERE Country IN ('Canada', 'Germany', 'France');
-- WHERE Country NOT IN ('Canada', 'Germany', 'France');  # 해당되지 않는 데이터도 가능
```
5. 테이블 customers에서 LastName 필드 값이 'son'으로 끝나는 데이터의 LastName, FirstName 조회
```sql
SELECT LastName, FirstName
FROM customers
WHERE LastName LIKE '%son';
```
6. 테이블 customers에서 FirstName 필드 값이 4자리면서 'a'로 끝나는 데이터의 LastName, FirstName 조회
```sql
SELECT LastName, FirstName
FROM customers
WHERE FirstName LIKE '___a';
```

#### Operators
Comparison Operators(비교 연산자)  
=, >=, <=, !=, IS, LIKE, IN, BETWEEN, AND, ...  
IN Operator
- 값이 특정 목록 안에 있는지 확인  
LIKE Operator
- 값이 특정 패턴에 일치하는지 확인(Wildcards와 함께 사용)
- Wildcard Characters
  - '%': 0개 이상의 문자열과 일치하는지 확인
  - '_': 단일 문자와 일치하는지 확인  
LIMIT clause
- 조회하는 레코드 수를 제한  

LIMIT syntax  
```sql
LIMIT [offset, ] row_count;
```
- 하나 또는 두개의 인자를 사용(0 또는 양의 정수)
- row_count는 조회하는 최대 레코드 수를 지정
![limit & offset](./img/limit_offset.PNG)

LIMIT Query 예시  
1. 테이블 tracks에서 TrackID, Name, Bytes 필드 데이터를 Bytes 기준 내림차순으로 7개만 조회
```sql
SELECT TrackId, Name, Bytes
FROM tracks
ORDER BY Bytes DESC
LIMIT 7;
-- LIMIT 3, 4 -- 4번째부터 7번째 데이터만 조회
```

Logical Operators  
AND, OR, NOT

### GROUP BY clause
레코드를 그룹화하여 요약본 생성  
집계 함수와 함께 사용  

집계 함수
- 값에 대한 계산을 수행하고 단일한 값을 반환하는 함수
- SUM, AVG, MAX, MIN, COUNT  

GROUP BY syntax
- FROM 및 WHERE 절 뒤에 배치
- GROUP BY 절 뒤에 그룹화 할 필드 목록을 작성

GROUP BY Query 예시  
1. Country 필드를 그룹화하고 COUNT 함수가 각 그룹에 대해 집계된 값 계산
```sql
SELECT Country, COUNT(*)
FROM customers
GROUP BY Country;
```

2. 테이블 tracks에서 Composer 필드를 그룹화하여 각 그룹에 대한 Bytes의 평균 값을 기준으로 내림차순 조회
```sql
SELECT Composer, AVG(Bytes)
-- SELECT Composer, AVG(Bytes) AS avgOfBytes
FROM tracks
GROUP BY Composer
ORDER BY AVG(Bytes) DESC;
-- ORDER BY avgOfBytes DESC;
```

3. 테이블 tracks에서 Composer 필드를 그룹화하여 각 그룹에 대한 Milliseconds의 평균 값이 10 미만인 데이터 조회(단, Milliseconds 필드는 60000으로 나눠 분 단위 값의 평균으로 계산)
```sql
SELECT Composer, AVG(Milliseconds / 60000) AS avgOfMinute
FROM tracks
GROUP BY Composer
HAVING avgOfMinute < 10;
```
HAVING clause
- 집계 항목에 대한 세부 조건을 지정
- 주로 GROUP BY와 함께 사용되며 GROUP BY가 없다면 WHERE처럼 동작

### SELECT statement 실행 순서
1. 테이블에서(FROM)
2. 특정 조건에 맞추어(WHERE)
3. 그룹화 하고(GROUP BY)
4. 만약 그룹화 조건이 있다면 맞추고(HAVING)
5. 조회하여(SELECT)
6. 정렬하고(ORDER BY)
7. 특정 위치의 값을 가져옴(LIMIT)

## Managing Tables

### Create a table

#### CREATE TABLE statement
테이블 생성  

CREATE TABLE syntax
- 각 필드에 적용할 데이터 타입 작성
- 테이블 및 필드에 대한 제약조건(constraints) 작성

```sql
CREATE TABLE table_name (
  column_1 data_type contraints, 
  column_2 data_type contraints, 
  ...
)
```
테이블 생성 예시  
```sql
CREATE TABLE examples (
  ExamId INTERGER PRIMARY KEY AUTOINCREMENT, 
  LastName VARCHAR(50) NOT NULL, 
  FirstName VARCHAR(50) NOT NULL
)
```

PRAGMA
- 테이블 schema 확인
```sql
PRAGMA table_info('examples');
```

SQLite 데이터 타입
1. NULL
- 아무런 값도 포함하지 않음을 나타냄
2. INTEGER
- 정수
3. REAL
- 부동 소수점
4. TEXT
- 문자열
5. BLOB
- 이미지, 동영상, 문서 등의 바이너리 데이터

Constraints  
- 제약 조건
- 테이블의 필드에 적용되는 규칙 또는 제한 사항
- 데이터의 무결성을 유지하고 데이터베이스의 일관성을 보장  

대표적인 제약 조건 3가지
1. PRIMARY KEY
- 해당 필드를 기본 키로 지정
- INTEGER 타입에만 적용되며, INT, BIGINT 등과 같은 다른 정수 유형은 적용되지 않음

2. NOT NULL
- 해당 필드에 NULL 값을 허용하지 않도록 지정

3. FOREIGN KEY
- 다른 테이블과의 외래 키 관계를 정의  


AUTOINCREMENT keyword
- 자동으로 고유한 정수 값을 생성하고 할당하는 필드 속성
- 주로 primary key 필드에 적용
- INTEGER PRIMARY KEY AUTOINCREMENT가 작성된 필드는 항상 새로운 레코드에 대해 이전 최대 값보다 큰 값을 할당
- 삭제된 값을 무시되며 재사용할 수 없게 됨


### Modifying table fields

#### ALTER TABLE statement
테이블 및 필드 조작

|명령어|역할|
|------|---|
|ALTER TABLE ADD COLUMN|필드 추가|
|ALTER TABLE RENAME COLUMN|필드 이름 변경|
|ALTER TABLE RENAME TO|테이블 이름 변경|

1. ALTER TABLE ADD COLUMN syntax
```SQL
ALTER TABLE table_name
ADD COLUMN column_definition;
```
- ADD COLUMN 키워드 이후 추가하고자 하는 새 필드 이름과 데이터 타입 및 제약 조건 작성
- 단, 추가하고자 하는 필드에 NOT NULL 제약조건이 있을 경우 NULL이 아닌 기본 값 설정 필요

ALTER TABLE ADD COLUMN 활용 예시
```SQL
ALTER TABLE examples
ADD COLUMN Country VARCHAR(100) NOT NULL DEFAULT 'default value';
```
```sql
ALTER TABLE examples
ADD COLUMN Age INTEGER NOT NULL DEFAULT 0;

ALTER TABLE examples
ADD COLUMN Address VARCHAR(100) NOT NULL DEFAULT 'default value';
```
- SQLite는 단일 문을 사용하여 한번에 여러 필드를 추가할 수 없음

2. ALTER TABLE RENAME COLUMN syntax
```SQL
ALTER TABLE table_name
RENAME COLUMN current_name TO new_name
```
- RENAME COLUMN 키워드 뒤에 이름을 바꾸려는 필드의 이름을 지정하고 TO 키워드 뒤에 새 이름을 지정

ALTER TABLE RENAME COLUMN 활용 예시
```SQL
ALTER TABLE examples
RENAME COLUMN Address TO PostCode;
```

3. ALTER TABLE RENAME TO syntax
```sql
ALTER TABLE table_name
RENAME TO new_table_name
```
- RENAME TO 키워드 뒤에 새로운 테이블 이름 지정

ALTER TABLE RENAME TO 활용 예시
```SQL
ALTER TABLE examples
RENAME TO new_examples;
```

### DROP TABLE

### Delete a table
DROP TABLE statement
- 테이블 삭제
```sql
DROP TABLE table_name;
```
- DROP TABLE statement 이후 삭제할 테이블 이름 작성

### 참고
타입 선호도(Type Affinity)
- 컬럼에 데이터 타입이 명시적으로 지정되지 않았거나 지원하지 않을 때 SQLite가 자동으로 데이터 타입을 추론하는 것  

SQLite 타입 선호도(Type Affinity)의 목적
1. 유연한 데이터 지원
- 데이터 타입을 명시적으로 지정하지 않고도 데이터를 저장하고 조회할 수 있음
- 컬럼에 저장되는 값의 특성을 기반으로 데이터 타입을 유추

2. 간편한 데이터 처리
- INTERGER Type Affinity를 가진 열에 문자열 데이터를 저장해도 SQLite는 자동으로 숫자로 변환하여 처리

3. SQL 호환성
- 다른 데이터베이스 시스템과 호환성을 유지  

반드시 NOT NULL 제약을 사용해야 하는가?
- 그렇지 않다.
- 하지만 데이터베이스를 사용하는 프로그램에 따라 NULL을 저장할 필요가 없는 경우가 많으므로 대부분 NOT NULL을 정의
- '값이 없다'라는 표현을 테이블에 기록하는 것은 '0'이나 '빈 문자열'등을 사용하는 것으로 대체하는 것을 권장

## Modifying Data

### Insert data

#### INSERT
테이블 레코드 삽입  

INSERT syntax
```sql
INSERT INTO table_name (c1, c2, ...)
VALUES (v1, v2, ...)
```
- INSERT INTO 절 다음에 테이블 이름과 괄호 안에 필드 목록 작성
- VALUES 키워드 다음 괄호 안에 해당 필드에 삽입할 값 목록 작성

INSERT 활용 예시
```SQL
INSERT INTO articles (title, content, createdAt)
VALUES ('title1', 'content1', '2000-01-01'), 
       ('title2', 'content2', '2001-01-01'), 
       ('title3', 'content3', '2002-01-01'), -- 여러개 한꺼번에 삽입 가능
       ('title4', 'content4', DATE());  -- DATE(): 현재 시간 추가
```

#### UPDATE
테이블 레코드 수정  

UPDATE syntax
```sql
UPDATE table_name
SET column_name = expression, 
[WHERE condition];
```
- SET 절 다음에 수정할 필드와 새 값을 지정
- WHERE절에서 수정할 레코드를 지정하는 조건 작성
- WHERE절을 작성하지 않으면 모든 레코드 수정  

UPDATE 활용 예시
1. articles 테이블 1번 레코드의 title 필드 값을 'update Title'로, content 필드 값을 'update Content'로 변경
```sql
UPDATE articles
SET title = 'update Title', content = 'update Content'
WHERE id = 1;
```

### Delete data
#### DELETE statement
테이블 레코드 삭제  

DELETE syntax
```sql
DELETE FROM table_name
[WHERE condition];
```
- DELETE FROM 절 다음에 테이블 이름 작성
- WHERE 절에서 삭제할 레코드를 지정하는 조건 작성
- WHERE 절을 작성하지 않으면 모든 레코드를 삭제  

UPDATE 활용 예시
1. articles 테이블에서 작성일이 오래된 순으로 레코드 2개 삭제
```sql
DELETE FROM articles
WHERE id in 
(
  SELECT id FROM articles
  ORDER BY createdAt
  LIMIT 2
);
```

### 참고
SQLite의 날짜와 시간
- SQLite에는 날짜 및 시간을 저장하기 위한 별도의 데이터 타입이 없음
- 대신 날짜 및 시간에 대한 함수를 사용해 표기 형식에 따라 TEXT, REAL, INTEGER 값으로 저장


## Multi table queries
### Join
관계
- 여러 테이블 간의 논리적 연결  

JOIN이 필요한 순간
- 테이블을 분리하면 데이터 관리는 용이해질 수 있으나 출력시에는 문제가 있음
- 테이블 한 개만 출력할 수 있어서 다른 테이블과 결합해서 출력하는 과정이 필요
- 이때 사용하는 것이 JOIN

### JOIN clause
둘 이상의 테이블에서 데이터을 검색하는 방법

1. INNER JOIN
두 테이블에서 값이 일치하는 레코드에 대해서만 결과를 반환
```SQL
SELECT select_list
FROM table_a INNER JOIN table b ON table_b.fk = table_a.pk
```
- FROM 절 이후 메인 테이블 지정
- INNER JOIN 절 이후 메인 테이블과 조인할 테이블을 지정(table_b)
- ON 키워드 이후 조인 조건을 작성
- 조인 조건은 table_a와 table_b 간의 레코드를 일치시키는 규칙을 지정  

INNER JOIN 활용 예시
- 1번 회원이 작성한 모든 게시글의 제목과 작성자명을 조회
```sql
SELECT articles.title, users.name
FROM articles INNER JOIN users on users.id = articles.userId
WHERE users.id = 1;
```

2. LEFT JOIN
오른쪽 테이블의 일치하는 레코드와 함께 왼쪽 테이블의 모든 레코드 반환
```SQL
SELECT select_list
FROM table_a LEFT JOIN table b ON table_b.fk = table_a.pk
```
- FROM 절 이후 왼쪽 테이블 지정(table_a)
- LEFT JOIN 절 이후 오른쪽 테이블 지정(table_b)
- ON 키워드 이후 조인 조건을 작성
  - 왼쪽 테이블의 각 레코드를 오른쪽 테이블의 모든 레코드와 일치시킴

LEFT JOIN 활용 예시  
- 1번 회원이 작성한 모든 게시글의 제목과 작성자명을 조회
![left join](./img/left_join.PNG)

  - 왼쪽 테이블의 모든 레코드를 표기
  - 오른쪽 테이블과 매칭되는 레코드가 없으면 NULL을 표시

- 게시글을 작성한 이력이 없는 회원 정보 조회
```SQL
SELECT *
FROM users LEFT JOIN articles ON articles.userId = users.id
WHERE articles.userId IS NULL;
```




## index
테이블의 동작 속도를 높여주는 자료 구조
- 대량의 데이터를 매번 정렬하면 프로그램이 느려질 수 밖에 없음