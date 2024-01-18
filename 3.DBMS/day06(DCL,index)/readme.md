## DCL(Data Controll Language)
- 데이터 제어어
- 데이터베이스에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령어

### DCL의 종류
- GRANT : 권한 부여
- REVOKE : 권한 강탈


### CMD로 오라클에 접속하여 존재하는 계정 검색

```SQL
SQLPLUS SYSTEM/1234

SELECT USERNAME FROM DBA_USERS;

USERNAME
------------------------------------------------------------
SYS
SYSTEM
ANONYMOUS
TEST_PM
APEX_PUBLIC_USER
FLOWS_FILES
APEX_040000
OUTLN
DIP
ORACLE_OCM
XS$NULL

USERNAME
------------------------------------------------------------
MDSYS
CTXSYS
DBSNMP
XDB
APPQOSSYS
HR
sj
```

### SCOTT계정 등록하기
```SQL
오라클 설치된 폴더에서 SCOTT 검색
SQL> @C:\oraclexe\app\oracle\product\11.2.0\server\rdbms\admin\scott.sql
SQL> SHOW USER;
USER is "SCOTT"

scott의 비밀번호는 TIGER로 되어 있기 때문에 소문자로 변경

SQL> alter user scott identified by tiger;

User altered.

SQL> conn scott/tiger;
Connected.
```


### SCOTT을 통해 BABY라는 아이디를 만드려 했지만 권한이 없어 실패
```
SQL> create user baby identified by baby;
create user baby identified by baby
                               *
ERROR at line 1:
ORA-01031: insufficient privileges

BABY라는 계정을 만들 때 BABY가 사용할 저장소를 만들어야 되는데 이 저장소를 테이블 스페이스라고 한다.

SQL> conn sys as sysdba
Enter password:
Connected.
SQL> show user
USER is "SYS"
SQL> select tablespace_name, status, contents from dba_tablespaces;

TABLESPACE_NAME                                              STATUS
------------------------------------------------------------ ------------------
CONTENTS
------------------
SYSTEM                                                       ONLINE
PERMANENT

SYSAUX                                                       ONLINE
PERMANENT

UNDOTBS1                                                     ONLINE
UNDO


TABLESPACE_NAME                                              STATUS
------------------------------------------------------------ ------------------
CONTENTS
------------------
TEMP                                                         ONLINE
TEMPORARY

USERS                                                        ONLINE
PERMANENT
------------------------------------------------------------------------------------
SQL> select file_name, tablespace_name,autoextensible from dba_data_files;
							ㄴ용량이 다 차면 자동으로 증가하는지
FILE_NAME
--------------------------------------------------------------------------------
TABLESPACE_NAME                                              AUTOEX
------------------------------------------------------------ ------
C:\ORACLEXE\APP\ORACLE\ORADATA\XE\USERS.DBF
USERS                                                        YES

C:\ORACLEXE\APP\ORACLE\ORADATA\XE\SYSAUX.DBF
SYSAUX                                                       YES

C:\ORACLEXE\APP\ORACLE\ORADATA\XE\UNDOTBS1.DBF
UNDOTBS1                                                     YES


FILE_NAME
--------------------------------------------------------------------------------
TABLESPACE_NAME                                              AUTOEX
------------------------------------------------------------ ------
C:\ORACLEXE\APP\ORACLE\ORADATA\XE\SYSTEM.DBF
SYSTEM

SQL> CREATE TABLESPACE BABY DATAFILE'C:\oraclexe\app\oracle\oradata\XE\BABY.DBF'SIZE 200M AUTOEXTEND ON NEXT 5M MAXSIZE 300M;

Tablespace created.

SQL> grant create user to scott; -> 스콧에게 계정을 만들 권한을 줌

Grant succeeded.

SQL> conn scott/tiger
Connected.
SQL> create user baby identified by baby;

User created.

SQL> conn baby/baby
ERROR: create session이라는 권한이 없어서 로그인이 안된다.
ORA-01045: user BABY lacks CREATE SESSION privilege; logon denied


Warning: You are no longer connected to ORACLE.
SQL> conn system/1111 -> system 계정으로 로그인하여 권한을 줘야 한다.
Connected.
SQL> grant create session to baby;

Grant succeeded.

SQL> conn baby/baby
Connected.

방금 만든 테이블스페이스와 baby 계정을 연결해야 한다. 안그러면 default 값인 user로 간다.
SQL> conn baby/baby
Connected.
SQL> alter user baby default tablespace BABY; -> 권한이 없으니 system으로 다시 로그인 하자.

 SQL> conn system/1111
Connected.
SQL> alter user baby default tablespace BABY; 디폴트 스페이스를 베이비로 바꿀거임

User altered.

SQL> alter user baby temporary tablespace TEMP; 임시저장소는 temp로 해놓겠다.

User altered.

SQL> alter user baby default tablespace BABY QUOTA unlimited on baby;
저 baby라는 계정이 baby테이블스페이스의 어느정도 양을 쓸꺼냐 무한으로 쓸거임
User altered.

SQL> conn baby/baby
Connected.
```

### BABY 계정으로 테이블 만들어보기
```SQL
SQL> create table test001(id varchar2(10), pw varchar2(10), age number, constraints baby_pk primary key(id));
create table test001(id varchar2(10), pw varchar2(10), age number, constraints baby_pk primary key(id))
*
ERROR at line 1:
ORA-01031: insufficient privileges --> 테이블 생성 권한이 없음

SQL> conn system/1111  
Connected.
SQL> grant create table to baby; -> 권한 줌

SQL> create table test001(id varchar2(10), pw varchar2(10), age number, constraints baby_pk primary key(id));

Table created. --> 테이블 생성 DBvear로 확인하기

SQL> select * from test001;

no rows selected

new connection -> oracle -> localhost, xe, baby,baby -> driver Setting
ok -> test
```