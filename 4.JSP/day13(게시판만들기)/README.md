# 게시판 만들기
- 게시판을 만들고 댓글처리 및 페이징 기능들을 추가해보도록 하자.

## 게시판 DB생성하기
```sql
--일련번호
create sequence seq_board_idx;

--게시판 DB
CREATE TABLE board(
	idx int,                          --게시판 일련번호
	name varchar2(100) not null,    --작성자
	subject varchar2(255) not null,  --게시판제목   
	content CLOB,                   --내용(씨롭.CharLargeObject)
	pwd varchar2(100),              --비번
	ip varchar2(100),                --ip
	regdate date,                    --작성일자
	readhit int default 0,            --조회수
	--계층형 게시판을 운영하기 위한 추가정보들
	ref int,  --기준글번호(댓글을 달기위한 메인글)
	step int, --댓글순서(수직적)
	depth int --댓글의깊이(댓글의 댓글 개념)
);

--idx를 고유키로 설정
alter table board add constraint pk_board_idx primary key(idx); 

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

## ref,step,depth에 대한 설명(댓글이 생성되는 원리)
- 계층형 게시판은 기본적으로 다음과같은 정렬구조를 갖는다.
- 게시판에서 최신글 순서로 보고싶다면... ref는 큰 값(최신글)기준, step은 작은값 기준
```sql
select * from board order by ref desc, step asc;
```
![image](https://user-images.githubusercontent.com/54658614/235058338-53fedc83-5977-440d-900e-b12a1797794e.png)

## Ex_날짜_board 프로젝트 생성하기
- DB,Mybatis,Ajax등의 환경을 구성하기 위한 설정파일 및 라이브러리 추가하기

![image](https://user-images.githubusercontent.com/54658614/235058401-67223fa7-7343-4f0f-95d5-da4c155466ae.png)



