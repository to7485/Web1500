# 아이디 중복 체크
- 테이블에서 id를 unique로 설정해놨기 때문에 중복되는 데이터를 넣으면 500에러가 발생한다.
- 하지만 고객의 입장에서 500에러가 뜨면 웹사이트가 고장이 났다고 생각할 것이다.

## 테이블 생성
```
--일련번호 관리객체
create sequence seq_myUser_idx;

--회원테이블
create table myUser(
  idx number(3) primary key, --일련번호
  name varchar2(100) not null, --이름
  id varchar2(100) not null, unique, --아이디(중복방지 unique)
  pwd varchar2(100) not null --비밀번호
);

--커밋
commit;
```

Ajax를 사용하기 위해 js폴더를 가져오고<br>
DB와 연결을 하기 위해 context.xml과 service패키지, 라이브러리들을 가져오자

## vo패키지에 UserVO 클래스 생성하기
```java

```
