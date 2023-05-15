# 방명록 만들기

## Ex_날짜_visit 프로젝트 만들기
- pom.xml과 resources에 있는 프로젝트 가져오기
    - Artifact Id, Name 수정해주기
- web,root,servlet.xml 삭제하기
- mapper이름 visit으로 바꾸고 mybatis-config.xml에 참조하기

## 방명록 테이블 작성하기
```sql
--방명록 DB

--시퀀스
create sequence seq_visit_idx;

create table visit(
  idx NUMBER(3) PRIMARY KEY,
  name VARCHAR2(50), --작성자
  content VARCHAR2(1000), --내용
  pwd VARCHAR2(50), --비번
  ip VARCHAR2(20), --ip
  regdate DATE --작성일
);

--샘플데이터
insert into visit values(
  seq_visit_idx.nextVal,
  '일길동',
  '내가 1빠',
  '1111',
  '192.1.1.1',
  sysdate
);

insert into visit values(
  seq_visit_idx.nextVal,
  '이길동',
  '내가 2빠',
  '1111',
  '192.1.1.1',
  sysdate
);

commit;
```
