# 생활코딩 Oracle

    강의 URL : https://opentutorials.org/course/3885


## 오라클 설치
- 구글 검색 `oracle express edition 18c` 검색 후 다운로드 진행
- 설치 후 `SQL Plus` 실행 후 아래와 같이 입력하여 해당 화면이 나온다면 제대로 설치가 완료된것
  
```sql
SQL*Plus: Release 18.0.0.0.0 - Production on 금 4월 15 13:08:02 2022
Version 18.4.0.0.0

Copyright (c) 1982, 2018, Oracle.  All rights reserved.

사용자명 입력: sys As SYSDBA
비밀번호 입력:

다음에 접속됨:
Oracle Database 18c Express Edition Release 18.0.0.0.0 - Production
Version 18.4.0.0.0

SQL>

```


## 스키마 (Schema)
- 연관된 표를 그룹핑 하는걸 말한다. 일종의 디렉토리를 말함

## 사용자
- 사용자를 생성하면 그 사용자에 해당되는 스키마가 생성된다.

### 사용자 생성
- `cmd`창을 실행 한 후 `sqlplus`라고 입력 후 `sys AS SYSDBA` 입력하여 관리자로 접속
- `CREATE USER 사용자명 IDENTIFIED BY 패스워드;` 

<img src="https://docs.oracle.com/cd/B19306_01/server.102/b14200/img/create_user.gif"><br>


```cmd
사용자명 입력: sys AS SYSDBA
비밀번호 입력:

다음에 접속됨:
Oracle Database 18c Express Edition Release 18.0.0.0.0 - Production
Version 18.4.0.0.0

SQL> CREATE USER brian IDENTIFIED BY 123qweasd;
CREATE USER brian IDENTIFIED BY 123qweasd
                                   *
1행에 오류:
ORA-00922: 누락된 또는 부적합한 옵션


SQL> CREATE USER brian IDENTIFIED BY 111111;
CREATE USER brian IDENTIFIED BY 111111
            *
1행에 오류:
ORA-65096: 공통 사용자 또는 롤 이름이 부적합합니다.


SQL> ALTER SESSION SET "_ORACLE_SCRIPT" = TRUE:
  2  ;
ALTER SESSION SET "_ORACLE_SCRIPT" = TRUE:
                                         *
1행에 오류:
ORA-00922: 누락된 또는 부적합한 옵션


SQL> ALTER SESSION SET "_ORACLE_SCRIPT" = TRUE;

세션이 변경되었습니다.

SQL> CREATE USER brian IDENTIFIED BY 111111;

사용자가 생성되었습니다.
```

### 사용자 권한 부여
- `GRANT DBA TO 사용자명`;
- 여기서 `DBA` 라는 키워드는 데이터베이스의 관리자 권한을 준다. 하지만 현실에서는 사용하지 않는다.

```sql
SQL> GRANT DBA TO brian;

권한이 부여되었습니다.

SQL> exit
Oracle Database 18c Express Edition Release 18.0.0.0.0 - Production
Version 18.4.0.0.0에서 분리되었습니다.

C:\Users\Brian>sqlplus

SQL*Plus: Release 18.0.0.0.0 - Production on 금 4월 15 13:29:35 2022
Version 18.4.0.0.0

Copyright (c) 1982, 2018, Oracle.  All rights reserved.

사용자명 입력: brian
비밀번호 입력:

다음에 접속됨:
Oracle Database 18c Express Edition Release 18.0.0.0.0 - Production
Version 18.4.0.0.0
```


### 테이블 생성
- 아래 명령어로 테이블 생성이 가능하며, 행의 제목과 타입설정이 가능하다.
```sql
SQL> CREATE TABLE topic (
  2  id NUMBER NOT NULL,
  3  title VARCHAR2(50) NOT NULL,
  4  description VARCHAR2(4000),
  5  created DATE NOT NULL
  6  );

테이블이 생성되었습니다.
```
- 생성한 테이블을 확인하고 싶을때 사용하는 명령어

```sql
SQL> SELECT table_name FROM all_tables WHERE OWNER = 'BRIAN';

TABLE_NAME
--------------------------------------------------------------------------------
TOPIC
```


### 행추가

```sql
SQL> INSERT INTO topic
  2  (id,title,description,created)
  3  VALUES
  4  (1,'ORACLE','ORACLE is ...', SYSDATE);

1 개의 행이 만들어졌습니다.

SQL> commit;

커밋이 완료되었습니다.
```


### 행 읽기
- `SELECT * FROM topic;`

### 행과 컬럼 제한하기
- 아이디, 제목,생성일자만 보고 싶다면?
  - `SELECT id, title, created FROM topic`;
- 아이디가 1인 것만 가져오고 싶다면?
  - `SELECT * FROM topic WHERE id = 1;`
- 아이디가 1보다 큰것만 가져오고 싶다면? 
  - `SELECT * FROM topic WHERE id > 1;`
- id값이 1이면서 아이디, 제목, 생서일자만 가져오고 싶다면?
  - `SELECT id, title, created FROM topic WHERE id = 1;`
- id값을 기준으로 오름차순으로 정렬을 하고 싶다면?
  - `SELECT * FROM topic ORDER BY id DESC;`
- id값을 기준으로 내림차순으로 정렬을 하고 싶다면?
  - `SELECT * FROM topic ORDER BY id ASC;`
- 페이징 기법, 0번쨰 이후 행들만 가져온다면? 
- OFFSET = 어디부터 가져올 것인가를 결정
- FETCH NEXT = 여기까지 가져온다.
  - `SELECT * FROM topic OFFSET 1 ROWS FETCH NEXT 2 ROWS ONLY`



### 행 수정
- 아래와 같이 수정한 후 꼭 commit을 해주어야 한다. 그래야 정상적으로 반영된다.
```sql
UPDATE topic
	SET
	 title = 'MSSQL',
	 description = 'MSSQL is...'
	WHERE
	 id = 3;
```


### 행 삭제
- 아이디 값이 3번만 삭제한다면 아래와 같은 명령어를 사용한다.
```sql
DELETE FROM topic WHERE id = 3;
```


### primary key (기본키)
- 표를 생성할 떄 추가할 수도 있고, 나중에도 추가할 수 있지만 가급적 표를 생성할 때 만드는 것이 좋음.
- 표 생성시 CONSTRAINT 명령어를 사용하여 이름을 정해주고 PRIMARY KET( 기준값)을 정해준다.
```sql
CREATE TABLE topic (
		id NUMBER NOT NULL,
		title VARCHAR2(20) NOT NULL,
		description VARCHAR2(2000),
		created DATE NOT NULL,
		CONSTRAINT PK_TOPIC PRIMARY KEY(id)
);
```


### join 사용하기
```sql
SELECT T.id TOPIC_ID, title, name FROM topic T
    LEFT JOIN author A ON T.author_id = A.id
;
```