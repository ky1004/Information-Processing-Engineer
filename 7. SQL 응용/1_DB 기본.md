# SQL 응용

> ## 1. 데이터베이스 기본

<br>

> ### 1) 트랜잭션

- 트랜잭션 : DB시스템에서 하나의 논리적 기능을 정상적으로 수행하기 위한 작업의 기본단위

  - 트랜잭션 특성 4가지
    - 원자성 (Atomicity)
    - 일관성 (Consistency)
    - 격리성 (Isolation)
    - 영속성 (Durability)

- 트랜잭션 상태변화 : 활동, 부분완료, 완료, 실패, 철회
- 트랜잭션 제어 언어(Translation Control Language) : 커밋, 롤백, 체크포인트

- 병행제어 : 다수 사용자 환경에서 여러 트랜잭션 수행 시 DB 일관성 유지를 위해 상호작용을 제어하는 기법

  - 병행제어 미보장 시 문제점
    - 갱신손실
    - 현황파악 오류
    - 모순성
    - 연쇄복귀

- 병행제어 기법

  - 로킹
  - 낙관적 검증
  - 타임스탬프 순서
  - 다중버전 동시성제어

- 고립화 수준 : 다른 트랜잭션이 현재의 데이터에 대한 무결성 보장을 위해 잠금을 설정하는 정도

  - 고립화 수준의 종류
    - Read Uncommitted
    - Read committed
    - Repeatable Read
    - Serializable Read

- 회복 기법 : 트랜잭션 수행 중 장애로 인해 손상된 DB를 손상 이전의 정상적 상태로 복구시키는 작업
  - 회복 기법 종류
    - 로그기반 : 즉시갱신(갱신결과 바로 DB반영), 지연갱신(완료 전까지 기록x)
    - 체크포인트 : 검사점 이후에 처리된 트랜잭션만 장애발생 이전 상태로 복구
    - 그림자페이징 : 장애발생 시 복제본을 이용하여 복구

<br>

> ### 2) DDL, DML, DCL

- SQL문법 : DB를 접근하고 조작하는데 필요한 표준언어를 활용할 수 있게 해주는 규칙

  - DDL : CREATE, ALTER, DROP, TRUNCATE
  - DML : SELECT, INSERT, UPDATE, DELETE
  - DCL : GRANT, REVOKE

- DDL의 개념 : 데이터를 담는 틀을 정의하는 언어
- DDL의 대상

  - 도메인 : 원자값들의 집합
  - 스키마 : 구조, 제약조건 등을 담은 기본적 구조
  - 테이블 : 저장공간(필드로 구성된 데이터집합체)
  - 뷰 : 가상 테이블
  - 인덱스 : 조회를 빠르게 하기 위한 색인

- 스키마의 유형 : 외부(사용자 뷰), 개념(전체 뷰), 내부(물리적 저장장치)

- 테이블

  - [물리/논리]
    - 테이블(Table) / 릴레이션(Relation),엔티티(Entity)
    - 행(Row) / 튜플(Tuple)
    - 열(Column) / 애트리뷰트(Attribute)
  - 식별자(Identifier) : 여러개의 집합체를 담고 있는 관계형DB에서 구분할 수 있는 논리적 개념
  - [개수]
    - 카디널리티(Cardinality) : 튜플 개수
    - 차수(Degree) : 애트리뷰트 개수
  - 도메인 : 하나의 애트리뷰트가 취할수 있는 같은 타입 원자값들의 집합

- 뷰 : 논리 테이블로서 사용자에게 테이블과 동일
  - 특징 : 변경 불가능, 논리적 데이터 독립성 제공, 데이터 조작 연산 간소화, 보안기능 제공
  - 목적 : 중요 데이터 일부만을 제공, 단순 질의어 사용가능, 다수의 테이블 대체가능
- 뷰 명령어 : CREATE VIEW Vwname AS Qy;

- 인덱스 : 데이터를 빠르게 찾을 수 있는 수단, 조회속도 증가
  (인덱스는 유일하며 중복이 있으면 안됨)
  - 종류 : 순서, 해시, 비트맵, 함수기반, 단일, 결합, 클러스터드
- 인덱스 명령어
  - CREATE [UNIQUE] INDEX Idxname ON Tbname(Clname);
  - DROP INDEX Idxname;
  - ALTER [UNIQUE] INDEX Idxname ON Tbname(Clname);
- 인덱스 스캔방식 : 범위(필요범위), 전체, 단일(수직적탐색), 생략(조건절빠져도 인덱스활용)

<br>

> > #### DDL

- CREATE, ALTER, DROP, TRUNCATE
- CREATE 테이블 속성
  - PRIMARY KEY
  - FOREIGN KEY
  - UNIQUE
  - NOT NULL
  - CHECK
  - DEFAULT
- ALTER : 컬럼수정, 컬럼명수정, 컬럼삭제
  - ALTER TABLE Tbname ADD Clname Dtype;
  - ALTER TABLE Tbname MODIFY Clname Dtype Constr;
  - ALTER TABLE Tbname DROP Clname;
  - ALTER TABLE Tbname RENAME COLUMN bf_Clname To af_Clname;
- DROP : 테이블 삭제 [CASCADE | RESTRICT]
  - DROP TABLE Tbname [CASCADE | RESTRICT];
- TRUNCATE : 테이블 내의 데이터 삭제 명령어

  - TRUNCATE TABLE Tbname;

- VIEW관련 DDL
  CREATE VIEW Vwname AS
  SELECT X FROM X WHERE X;
  (SELECT문 조회쿼리)
- CREATE OR REPLACE VIEW Vwname As Qy;
  - REPLACE : 뷰를 교체
- DROP VIEW Vwname;

- INDEX 관련 DDL
  CREATE INDEX Idxname ON Tbname(Clname1, Clname2, . . .);
  ALTER INDEX Idxname ON Tbname(Clname1, Clname2, . . .);
  DROP INDEX Idxname;

- View As / Index On

<br>

> > #### DML

- 데이터베이스에 저장된 자료들을 입력, 수정, 삭제, 조회하는 언어
- DML 유형 : SELECT, INSERT, UPDATE, DELETE
- SELECT 명령문
  - SELELCT FROM WHERE GROUP BY HAVING ORDER BY
- WHERE조건
  - 비교 : =,<>,<,<=,>,>=
  - 범위 : BETWEEN A AND B
  - 집합 : IN, NOT IN
  - 패턴 : LIKE
  - NULL : IS NULL, IS NOT NULL
  - 복합조건 : AND, OR, NOT
