# 성적관리 프로그램
- view를 만들어서 없는 컬럼을 사용하는법에 대해서도 알아보자.

## Ex_날짜_SungJuk 프로젝트 생성하기
- context.xml, 라이브러리 넣어주기
- service 패키지 넣어주기

### 성적테이블 만들기
```
--시퀀스 생성
create sequence seq_sungtb_no;

--테이블 생성
create table sungtb
(
   no int primary key,    //no라는 이름의 기본키        
   name varchar2(100) not null,  //이름(빈값을 허용하지 않는다)
   kor  NUMBER(3) not null ,    //국어성적(기본값0) number 써도 됨 유효성체크할 때
   eng  NUMBER(3) not null,	빈칸이면 경고 띄울거기 때문에 not null 안해도 됨    
   mat  NUMBER(3) not null	//영어성적(기본값0)         
   //수학성적(기본값0)         
);

--샘플 데이터
insert into sungtb values(seq_sungtb_no.nextVal,'일길동', 77, 88, 99);

insert into sungtb values(seq_sungtb_no.nextVal,'이길동',97,98,99);

insert into sungtb values(seq_sungtb_no.nextVal,'삼길동',87,88,89);

commit;
```
