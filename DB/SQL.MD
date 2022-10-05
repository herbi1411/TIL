## SQL

---

- Structured Query Language
- RDBMS를 관리하기 위해 만들어진 특수 목적의 프로그래밍 언어

## SQL Commands

---

- DDL (Data Definition Language) - 데이터 정의 언어
    - 관계형 데이터베이스 구조를 정의하기 위한 언어
    - `CREATE` `DROP` `ALTER`
- DML (Data Manipulation Language) - 데이터 조작 언어
    - 데이터를 조작 (생성, 조회, 변경, 삭제)하기 위한 명령어
    - `INSERT` `SELECT` `UPDATE` `DELETE`
- DCL (Data Control Language) - 데이터 제어 언어
    - 데이터의 보안, 수행제어, 사용자 권한 부여 등을 정의 하기 위한 명령어
    - `GRANT` `REVOKE` `COMMIT` `ROLLBACK`

## SQL Syntax

---

- 모든 SQL문은 키워드로 시작하고, 하나의 statement는 세미콜론으로 끝난다.
- SQL 키워드는 대소문자를 구분하지 않는다. (but 대문자로 쓸 것을 권장👊)
- statement : 독립적으로 실행할 수 있는 완전한 코드 조각
- clause : state의 하위 단위(절) (ex. select, from, where 각각)

## SQLite Data Types

---

1. NULL
2. INTEGER
3. REAL
4. TEXT
5. BLOB (Binary Large Object)

## SQLite Datatypes 특징

---

1. 동적 타입 시스템(dynamic type system) 사용
    - 컬럼에 선언된 데이터 타입에 의해서가 아니라, 컬럼에 저장된 값에 따라 데이터 타입이 결정됨
2. 테이블 생성 시 컬럼의 데이터 타입을 선언하지 않아도 됨
    - 동적 타입 시스템 덕분에 컬럼값을 적절한 데이터타입으로 지정함
    - 테이블을 유연하게 만들 수 있음. (but 오히려 안좋은 영향도 있음 (일관성))
    - 다른 DBMS와 호환성 문제 발생 가능, 따라서 타입을 지정하는 것을 권장⚠️
3. 데이터 타입을 지정하면, SQLite는 입력된 데이터의 타입을 지정된 데이터 타입으로 변환

Type Affinity → 타입 선호도 (SQLite에 지정된 5가지 외의 데이터 타입을 선언하면, 내부적으로 각 타입의 지정된 선호도에 따라 5가지 선호도로 인식됨

## Constraints

---

- 데이터베이스 내의 데이터에 대한 정확성, 일관성을 보장하기 위해 데이터 변경, 혹은 수정 시 여러 제한을 두어 데이터의 정확성을 보증하는 것

종류

1. `NOT NULL`
2. `UNIQUE`
3. `PRIMARY KEY`
    - NOT NULL을 암시적으로 포함
    - INTEGER 타입에만 사용 가능
4. `AUTOINCREMENT`
    - 사용되지 않는 값이나, 이전에 삭제된 행의 값 재사용을 방지
    - 테이블을 생성할 때 `rowid` 컬럼이 생성 돼 값이 차례대로 암시적 자동 증가 (1에서 시작)
    - `rowid` 가 시스템 할당량을 초과하면, 지워진 id를 찾아서 할당, 지워진 id도 없다면 `SQLITE_FULL` 에러가 발생
5. 기타등등

## ALTER TABLE

---

예시

```sql
ALTER TABLE A RENAME TO NEW_A; // 테이블 이름 변경

ALTER TABLE A RENAME COLUMN col_a TO col_b; // 컬럼 이름 변경

ALTER TABLE A ADD COLUMN col_c; // 열 추가

ALTER TABLE A DROP COLUMN col_c; // 열 삭제
```

삭제하지 못하는 경우

1. 컬럼이 다른 곳에서 참조되는 경우 (외래키 제약조건에서 사용되는 경우)
2. `PRIMARY KEY` 인 경우
3. `UNIQUE` 제약 조건이 있는 경우

## DML

---

`DISTINCT` : 조회 결과에서 중복된 행을 제거

## Filtering Data

---

`LIKE` 

- 대소문자 구분하지 않음
- 와일드카드 2개
    1. % (percent)
        - **0개 이상**의 문자가 올 수 있음을 의미
    2. _ (underscore)
        - **단일(1개)** 문자가 있음을 의미

`LIMIT`

- 쿼리에서 반환되는 행 수를 제한
- SELECT 문에 선택적으로 사용할 수 있는 절
- `OFFSET` 을 같이 사용하면 지정한 OFFSET부터 LIMIT 수만큼 데이터를 가져옴

## SQLite3 csv 파일 읽어오기

---

```bash
sqlite3 <dbname>

.mode csv
.import <csvname> <tablename>
```