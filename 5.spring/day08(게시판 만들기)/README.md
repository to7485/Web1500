# 게시판 만들기

## Ex_날짜_SpringBoard (SpringLegacy Project) 생성하기
- com.korea.bbs
- pom.xml, resources 패키지들 가져오기
- jsp에서 썼던 board.xml, util패키지 붙혀넣기

```java
package util;

public class Common {
	
	//일반게시판용
	public static class Board{
  
		////// 한줄 추가하기
		public final static String VIEW_PATH = "/WEB-INF/views/board/";
		/////
		//한페이지에 보여줄 게시물 개수
		public final static int BLOCKLIST = 10;
	
		//페이지 메뉴 수
		// <- 1 2 3 ->
		public final static int BLOCKPAGE = 3;
	}
	
	//공지사항 게시판용
	public static class Notice{
		//한 페이지에서 보여줄 게시물 수
		public final static int BLOCKLIST = 20;
		public final static int BLOCKPAGE = 5;
	}
}

```

## vo패키지에 BoardVO클래스 만들기

```java
package vo;

import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class BoardVO {
	
	private int idx; //글 번호
	private int readhit; //조회수
	private int ref; //참조글 번호
	private int step; //순서
	private int depth; //댓글 깊이
	private int del_info; //삭제여부
	
	private String name; //작성자
	private String subject; //제목
	private String content; //내용
	private String pwd; //비밀번호
	private String  ip; //ip
	private String regdate; //작성날짜

}
```

## dao 패키지에 BoardDAO클래스 생성하기

```java
package dao;

import java.util.HashMap;
import java.util.List;

import org.apache.ibatis.session.SqlSession;

import vo.BoardVO;

public class BoardDAO {

	SqlSession sqlSession;
	
	public BoardDAO(SqlSession sqlSession) {
		this.sqlSession = sqlSession;
	}
	
	//페이지별로 게시글 조회
	public List<BoardVO> selectList(HashMap<String, Integer> map){
		
		List<BoardVO> list = sqlSession.selectList("b.board_list", map);
		sqlSession.close();
		return list;
	}
	
	//게시글 추가
	public int insert(BoardVO vo) {
		
		int res = sqlSession.insert("b.board_insert", vo);
		
		sqlSession.close();
		return res;
	}
	
	//idx에 해당하는 상세 내용 한건 조회
	public BoardVO selectOne(int idx) {
		
		BoardVO vo = sqlSession.selectOne("b.board_one",idx);
		sqlSession.close();
		return vo;
	}
	
	//조회수 증가
	public int update_readhit(int idx) {
		
		int res = sqlSession.update("b.update_readhit",idx);
		sqlSession.close();
		return res;
	}
	
	//댓글추가를 위한 step+1
	public int update_step(BoardVO vo) {
		
		int res = sqlSession.update("b.board_update_step", vo);
		sqlSession.close();
		return res;
	}
	
	//댓글추가
	public int reply(BoardVO vo) {
		int res = sqlSession.insert("b.board_reply",vo);
		sqlSession.close();
		return res;
	}
	
	//게시글삭제(된것처럼 업데이트)
	public int del_update(BoardVO vo) {
		int res = sqlSession.update("b.del_update",vo);
		sqlSession.close();
		return res;
	}
	
	//전체 게시물 수 조회
	public int getRowTotal() {
		int count = sqlSession.selectOne("b.board_count");
		sqlSession.close();
		return count;
	}
}

```










