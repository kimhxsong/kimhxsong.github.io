- SQL 명령어 종류
	- DDL - 데이터 구조 정의
		- CREATE: 데이터 구조 생성
		- ALTER: 기존 데이터 구조 수정
		- **DROP**: 데이터 구조 삭제
		- **TRUNCATE**: 테이블의 모든 데이터를 삭제 (구조 유지, 롤백 불가)
		- RENAME: 데이터베이스 객체 이름 변경
	- DML
		- SELECT: 데이터 조회
		- INSERT: 데이터 삽입
		- DELETE: 데이터 삭제 (조건식, 롤백 가능)
		- UPDATE: 데이터 수정
	- DCL
		- GRANT: 특정 사용자에게 권한을 부여
		- REVOKE: 특정 사용자에게 권환을 회수
	- TCL
		- COMMIT: 트랜잭션의 작업 내용을 영구 반영
		- ROLLBACK: 트랙잭션 중의 작업을 취소하고 이전 상태로 복구
		- SAVEPOIN: 롤백할 수 있는 중간 지점 지정
	-
- 비절차적 데이터 조작어? 절차적 데이터 조작어?
	- SQL은 '선언적' 언어로 사용자가 '무엇을' 원하는지만 명시한다. '선언적' 이라는 말은 '비절차적' 으로 바꿔쓸 수 있다.
	- 절차적 데이터 조작어는 프로그래밍 언어 C/C++ , Java 등 프로그래밍 언어를 통해 구현되고 레코드 단위로 처리한다.
	-
- 호스트 프로그램 속에 삽입되어 사용되는 DML 명령어들을 데이터 부속어(Data Sub Language)라고 한다.
	- 호스트언어(C, Java)에 내장되어 사용하고, 데이터 베이스 조작을 위한 명령어이다.
	- 내장 SQL(Embedded SQL이라고도 한다.
	- 타입스크립트에서 사용하는 ORM(Object-Relational Mapping) 모듈도 데이터 부속어의 현대적인 형태로 볼 수 있다. 다만 과거와 달리 객체지향적 방식으로 추상화 되었고, Type-safe 하며 더 높은 수준의 추상화를 제공한다.
-
- 오라클과 SQL Server에서의 CREATE TABLE, PRIMARY KEY, NOT NULL, CONSTRAINT 정의 방식
	- 열 정의 시 제약 조건을 바로 설정하는 방식
	  logseq.order-list-type:: number
		- PRIMARY KEY 를 바로 설정하면 UNIQUE, NOT NULL 을 명시할 필요가 없다.
	- CONSTRAINT 키워드로 따로 명시하는 방식
	  logseq.order-list-type:: number
		- CONTRAINT __PK이름__ PRIMARY KEY (__열이름__)
		- CONTRAINT __CHECK제약이름__ CHECK (__표현식__)
	- 테이블 생성 후 ALTER TABLE로 제약 조건 추가
	  logseq.order-list-type:: number
		- 오라클: ALTER TABLE __테이블명__ MODIFY __열이름__ __타입(호환가능해야함)__ NOT NULL;
		- SQL Server: ALTER TABLE __테이블명__ ALTER COLUMN __열이름__ __타입(호환가능해야함)__ NOT NULL;
		- ALTER TABLE __테이블명__ ADD CONSTRAINT __PK명__ PRIMARY KEY (__열이름__);
	-
- ALTER TABLE ...  오라클과 SQL Server 차이
	- 오라클은 괄호 안에 여러 컬럼을 쉼표로 나열해서 동시에 수정할 수 있지만,
	- SQL Server는 한 번에 한 열만 수정 가능하고 괄호를 사용하지 않는다.
-
- NULL과 모든 비교는 알 수 없음을 반환한다.
	- 오라클과 SQL Server의 처리 방식
		- | **비교식** | **결과**| **설명** |
		  | ---- | ---- | ---- | ---- | ---- |
		  | NULL = NULL | UNKNOWN | 둘 다 값이 없으므로 같다고 할 수 없음|
		  | NULL <> NULL | UNKNOWN | 다르다고도 할 수 없음 |
		  | NULL = 5 | UNKNOWN | 5인지 아닌지도 모름 |
		- | **비교식** | **Oracle 결과** | **SQL Server 결과** | **설명** |
		  | ---- | ---- | ---- | ---- | ---- |
		  | NULL = NULL | UNKNOWN → 조건식에서는 FALSE처럼 작동 | 동일 | 일반 WHERE 조건에서 해당 행은 필터링됨|
		  | NULL <> NULL | UNKNOWN → 조건식에서는 FALSE처럼 작동 | 동일 | 마찬가지로 필터링됨 |
		  | SELECT ... WHERE column = NULL | 결과 없음 | 결과 없음 | IS NULL 사용해야 함 |
		  | SELECT ... WHERE column <> NULL | 결과 없음 | 결과 없음 | IS NOT NULL 사용해야 함 |
	-
- CREATE TABLE 열 정의부에서 PK, FK 를 정의하지 않는 이유
	- **제약 조건 이름을 지정할 수 없다.**
		- 시스템이 자동으로 외래키 제약조건 이름을 생성한다.
		- 나중에 삭제하거나 수정할 때 이름을 모르거나 추적하기 어렵고,
		- 에러 메시지에 의미 없는 시스템명이 등장한다.
	- **복합 키 정의가 불가능하다.**
		- 복합 PK, FK  정의는 반드시 제약 정의부에서만 가능
	-
- 인덱스 생성
	- CREATE INDEX __인덱스명__ ON __테이블명 (컬럼1, 컬럼2, ..., 컬럼N)__;
-
- PK는 기본적으로 NOT NULL이다. 그렇다면 FK도 NULL 값을 가질 수 없을까?
	- ❌ 외래키(FK)는 기본키(PK)뿐 아니라 대체키(UNIQUE 제약이 있는 컬럼) 도 참조할 수 있다.
	- 하지만 UNIQUE 제약이 있다고 해서 NOT NULL인 것은 아니다.
	- 외래키를 정의할 때 명시적으로 NOT NULL 제약을 설정하지 않으면 NULL이 허용된다.
	  	•	이때 참조 대상 컬럼이 NOT NULL 제약을 갖고 있더라도, 자식 테이블의 외래키 컬럼에 NULL이 들어가면 참조 무결성 검사는 생략된다. (즉, 무결성 위반 아님)
-
- 테이블 불필요한 컬럼 삭제
	- ALTER TABLE __테이블명__ DROP COLUMN __컬럼명__;
	-
- 테이블 이름 변경하는 방법
	- RENAME __현재_객체명__ TO __새로운_객체명__
	- ANSI 표준 기준,  오라클과 동일
-
- FK 참조 동작
	- ON DELETE / ON UPDATE
		- CASCADE: 테이블도 함께 삭제/수정
		- RESTRICT: 부모 테이블에 참조된 데이터가 있으면 삭제/수정 거부
		- NO ACTION: RESTRICT와 유사하지만 제약조건을 statement 종료 시점에 평가
		- SET NULL
		- SET DEFAULT
	- AUTOMATIC? DEPENDENT?
		- Insert Action: Automatic, Set Null, Set Default, Dependent, No Action
		- Automatic
			- 의미: 자식(Child) 레코드를 삽입하려 할 때, 부모(Master) 테이블에 **참조 대상이 되는 PK가 존재하지 않으면**, 자동으로 부모 레코드를 **생성**해줌.
			- 전제 조건:
				- 마스터 테이블에 해당 PK(또는 유니크 키)가 **없어야** 하며,
				- 다른 칼럼들도 **NULL 허용**이거나 **DEFAULT 값이 지정**되어 있어야 정상 작동함.
				- 그렇지 않으면 부모 테이블에 레코드를 삽입할 수 없어서 참조 무결성 위반이 됨.
				- 예:
				  
				  ```sql
				  -- 자식에 외래키 추가 시 부모 키가 없으면 부모를 자동 삽입
				  INSERT INTO child (parent_id, ...) VALUES (5, ...);  
				  -- => parent_id = 5인 부모 레코드가 없으면 자동으로 parent에 5를 넣고 생성
				  ```
		- Dependent
			- 의미: 자식(Child) 레코드는 **부모 테이블에 해당 PK가 존재할 때만** 삽입이 허용됨.
			- 즉, 부모 테이블이 반드시 선행되어 있어야 함.
			- 여기서 말하는 “PK”는 **자식 테이블에서 외래키(FK)로 참조 중인 부모의 PK**를 뜻함.
			- 예:
			- ```sql
			  -- parent 테이블에 id=5가 있어야만 자식이 삽입 가능
			  INSERT INTO child (parent_id, ...) VALUES (5, ...);  
			  -- parent에 5가 없으면 에러 발생
			  ```
		- | 옵션       | 부모 PK 존재 여부 | 자식 삽입 시 동작                             |
		  | Automatic  | 없어도 됨          | 부모 자동 생성 (단, 기본값 조건 충족 시)     |
		  | Dependent  | 있어야 함          | 없으면 에러 발생, 부모 먼저 삽입해야 함      |
		- **RDBMS에서 외래키(Foreign Key) 제약조건을 설정할 때, automatic이나 dependent라는 동작 모드를 명시적으로 지정하는 건 존재하지 않음.**
			- 이건 논리 모델링 툴이나 일부 ORM, 혹은 데이터 모델링 개념에서 설명할 때 쓰는 용어들
		- ==기본적으로는 RDBMS에서는 Dependent처럼 동작한다==. 즉, **부모 키가 있어야 자식 삽입 가능**.
		- Automatic  동작은? RDBMS에서는 기본적으로 지원하지 않음.
		- 자동으로 부모 테이블에 해당 키를 생성해주는 기능은 일반적인 SQL 표준에는 없음
		- 이런 기능은:
			- 일부 ORM 프레임워크에서 **코드 레벨에서 처리**
			- 또는 트리거(Trigger)로 직접 구현해야 함
			- 예: PostgreSQL에서 BEFORE INSERT 트리거로 자동 생성할 수 있음.
		- 요약
		  | 모드       | 실제 SQL FK 기본 동작? | 비고                                         |
		  | Dependent  | ✅ 기본 동작            | 부모 키 없으면 삽입 실패                     |
		  | Automatic  | ❌ 기본 미지원          | ORM/트리거/애플리케이션 로직으로 구현 가능   |
	-
-