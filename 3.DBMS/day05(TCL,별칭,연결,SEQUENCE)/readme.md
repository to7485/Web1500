# TCL(Transaction Control Language): (DML을 위한 명령어) 트랜젝션 제어 언어
- 트랜잭션 : 데이터베이스의 작업을 처리하는 논리적 연산 단위[분리될 수 없는 최소 단위]
    - 어떠한 작업을 하기 위해서는 Select ,Insert, Update 를 사용해야 하는데 이걸 하나의 작업 단위라고 한다.


## 트랜잭션의 특성
- 원자성(Atomicity) : 원자와 같이 데이터베이스 연산들이 나눌수도, 줄일수 없는 하나의 유닛으로서 취급됨.
    - 트랜잭션에 정의된 연산들은 모두 성공적으로 실행되던지 아니면 전혀 실행되지 않은 상태로 남아야 함

```SQL
10,000원이 든 계좌 A에서 0원이 든 계좌 B로 돈 10,000원을 입금한다.
-- query1
UPDATE 계좌
SET 잔액 = 0
WHERE 계좌번호 = A;
-- query2
UPDATE 계좌
SET 잔액 = 10000
WHERE 계좌번호 = B;
```
  
  
- 일관성(Consistency) : 데이터베이스의 트랜잭션이 제약조건, cascades, triggers를 포함한 정의된 모든 조건에 맞게 데이터 값이 변경됨
    - 트랜잭션 실행 전의 데이터베이스 내용이 잘못되지 않으면 트랜잭션 실행 후에도 데이터베이스 내용이 잘못되면 안됨

- 고립성(Isolation) : 특정 DBMS에서 다수의 유저들이 같은 시간에 같은 데이터에 접근하였을 때 수행중인 트랜잭션이 완료될 때 까지<br> 다른 트랜잭션의 요청을 막음으로서 데이터가 꼬이는걸 방지한다.
    - 트랜잭션 실행 도중에 다른 트랜잭션의 영향을 받아 잘못된 결과를 만들면 안됨

- 영속성,지속성(Durability) : 트랜잭션 실행이 성공적일 때, 그 트랜잭션이 갱신한 데이터베이스 내용은 영구적으로 저장됨.


## TCL의 종류

- COMMIT : DML로 변경된 데이터를 데이터베이스에 적용할 때 사용
  - INSERT,UPDATE,DELETE문 등을 사용한 후 변경 작업을 데이터베이스에 반영하기 위해 사용
  - COMMIT문 사용시 이전 데이터는 영원히 지워짐
  - COMMIT문 사용시 모든 사용자가 변경된 데이터 확인 가능
  - SQLServer는 기본적으로 AUTO COMMIT모드이므로 자동으로 COMMIT을 수행한다.<br> (DML문이 성공하면 자동으로 COMMIT을 실행한다.)
  - COMMIT 실행시 하나의 트랜잭션 과정을 종료<br> → COMMIT이 실행되기 전에 다른 사용자가 완료되지 않은 데이터를 보거나 변경할 수 없음<br>

사용법 : COMMIT;<br>

- ROLLBACK : DML로 변경된 데이터를 변경 이전 상태로 되돌릴 때 사용
    - 데이터에 대한 변경사항 취소
    - ROLLBACK문 사용시 이전 데이터 다시 재저장 -> COMMIT되지 않은 상위 트랜잭션을 모두 ROLLBACK시킴
    - ROLLBACK문 사용시 관련 행의 잠금(LOCKING)이 풀려 다른 사용자들이 데이터 변경 가능
    - SQL Server는 기본적으로 AUTO COMMIT모드이므로 자동으로 COMMIT, 오류가 발생하면 자동으로 ROLLBACK처리

사용법 : ROLLBACK;<br>

- SAVEPOINT : 오류 복구 처리에 효과적인 방법 -> 전체 트랜잭션을 ROLLBACK하지 않고도 오류 복귀 가능
    - 효과적으로 오류 복구 처리를 위해 저장점을 저장할 때 사용<br> -> 전체 트랜잭션을 ROLLBACK하지 않고 현시점에서 SAVEPOINT까지 일부 트랜잭션만 오류 복귀 가능
    - 복잡한 대규모 트랜잭션에서 에러가 발생했을 때 주로 사용
    - 복수의 저장점 정의 가능
    - 저장점까지 롤백할 때는 "ROLLBACK TO 저장점;" 사용

사용법 : SAVEPOINT 세이브포인트이름;<br>
ROLLBACK TO 세이브포인트이름;<br>

```SQL
디비버는 AUTO COMMIT 상태가 default이다.
위 작업표시줄에 T 모양을 눌러 수동 COMMIT으로 바꾸자.

-- EMPLOYEES 테이블에서 JOB_ID가 'IT_PROG'인 사람들의 이름을 자신의 이름으로 바꾸기
UPDATE EMPLOYEES
SET FIRST_NAME = '이현준'
WHERE JOB_ID = 'IT_PROG';
```









