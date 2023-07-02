# 게시판 만들기

## Ex_날짜_board 프로젝트 생성하기
- resources에 config패키지,mapper패키지 생성하기
- java에 controller,dao,vo,util 패키지 만들기

## 게시판DB생성하기
```sql
--일련번호
create sequence seq_board_idx;

--게시판 DB
CREATE TABLE board(
	idx number(3) primary key,      --게시판 일련번호
	name varchar2(100) not null,    --작성자
	subject varchar2(255) not null,  --게시판제목   
	content CLOB,                   --내용(씨롭.CharLargeObject)
	pwd varchar2(100),              --비번
	ip varchar2(100),                --ip
	regdate date,                    --작성일자
	readhit number(3) default 0,     --조회수
	--계층형 게시판을 운영하기 위한 추가정보들
	ref int,  --기준글번호(댓글을 달기위한 메인글)
	step int, --댓글순서(수직적)
	depth int --댓글의깊이(댓글의 댓글 개념)
);

--샘플데이터 추가(메인글1 -> 댓글1)
--새글쓰기
insert into board values(
		seq_board_idx.nextVal, 
		'이름-일길동', 
		'제목-내가일등!',
		'내용-..는꿈', 
		'1234', 
		'192.0.0.1', 
		sysdate, 
		0, 
		seq_board_idx.currVal,  -- 현재 시퀀스값 
		0, 
		0);

--댓글쓰기
insert into board values(
		seq_board_idx.nextVal, 
		'이름-이길동', 
		'제목-난2등ㅋ',
		'내용-ㅋㅋㅋㅋㅋㅋ', 
		'1234', 
		'192.0.0.2', 
		sysdate, 
		0, 
		1, --메인글의 idx
		1, -- step
		1 -- depth
);

--댓글의 댓글
insert into board values(
		seq_board_idx.nextVal, 
		'삼길동', 
		'오예3등',
		'난삼등', 
		'1234', 
		'192.0.0.3', 
		sysdate, 
		0, 
		1, --메인글의 idx
		2, -- step
		2 -- depth
);

commit;
```
```
