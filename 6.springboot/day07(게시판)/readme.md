# 게시판 만들기

## Ex_날짜_board 프로젝트 생성하기
- resources에 config패키지,mapper패키지 생성하기(config.xml,mapper.xml 복사하기)
  - static에 img폴더 복사해서 넣기
- java에 controller,dao,vo패키지 만들기



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
## mapper패키지에 BoardMapper 클래스 생성하기
```java
package com.example.boatd.mapper;

import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface BoardMapper {

}

```

## dao패키지에 BoardDAO클래스 생성하기
```java
package com.example.boatd.dao;

import org.springframework.stereotype.Repository;

import com.example.boatd.mapper.BoardMapper;

import lombok.RequiredArgsConstructor;

@Repository
@RequiredArgsConstructor
public class BoardDAO {

	private final BoardMapper boardMapper;
	
	
}

```

## controller패키지에 BoardController클래스 만들기
```java
package com.example.boatd.controller;

import org.springframework.stereotype.Controller;

import com.example.boatd.dao.BoardDAO;

import lombok.RequiredArgsConstructor;

@Controller
@RequiredArgsConstructor
public class BoardController {
	
	private final BoardDAO board_dao;
	
	
	
}

```

# 조회하기
- 페이지별 게시물 조회하기

## BoardMapper에 메서드 작성하기
```java
package com.example.boatd.mapper;

import java.util.HashMap;
import java.util.List;

import org.apache.ibatis.annotations.Mapper;

import com.example.boatd.vo.BoardVO;

@Mapper
public interface BoardMapper {

	//페이지 게시물 조
	public List<BoardVO> selectList(HashMap<String, Integer> map);
	
	//전체 게시물 수 조회
	public int getRowTotal();
}

```

## config.xml에 별칭 작성해주기
```xml
<?xml version="1.0" encoding="UTF-8"?>
	  
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0/EN" "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<typeAliases>
		<typeAlias type="com.example.boatd.vo" alias="boardVO"/>
	</typeAliases>
</configuration>
```

## board.xml에 쿼리문 작성하기
```xml
<?xml version="1.0" encoding="UTF-8"?>
	  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0/EN" "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.boatd.mapper.BoardMapper">
	<!-- 페이지별 게시물 조회 -->
	<select id="board_list" resultType="board">
		select * from
		(select rank() over(order by ref desc,step) no, b.*
		from board b)
		where no between #{start} and #{end}
	</select>

	<!-- 전체 게시물 개수 조회 -->
	<select id="board_count" resultType="int">
		select count(*) from board
	</select>
</mapper>
```

## BoardDAO에 메서드 작성하기
```java
package com.example.boatd.dao;

import java.util.HashMap;
import java.util.List;

import org.springframework.stereotype.Repository;

import com.example.boatd.mapper.BoardMapper;
import com.example.boatd.vo.BoardVO;

import lombok.RequiredArgsConstructor;

@Repository
@RequiredArgsConstructor
public class BoardDAO {

	private final BoardMapper boardMapper;
	
	public List<BoardVO> selectList(HashMap<String,Integer> map){
		return boardMapper.selectList(map);
	}
	
	public int getRowTotal() {
		return boardMapper.getRowTotal();
	}
}

```

## BoardController에 매핑 잡아주기
```java
package com.example.boatd.controller;

import java.util.HashMap;
import java.util.List;

import org.springframework.http.HttpRequest;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import com.example.boatd.common.Common;
import com.example.boatd.dao.BoardDAO;
import com.example.boatd.util.Paging;
import com.example.boatd.vo.BoardVO;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpSession;
import lombok.RequiredArgsConstructor;

@Controller
@RequiredArgsConstructor
@RequestMapping("/board/*")
public class BoardController {
	
	private final BoardDAO board_dao;
	private final HttpServletRequest request;
	
	@GetMapping("board_list.do")
	public String list(Model model, String page) {
		int nowPage = 1;
		
		//board_list.do <-- null
		//board_list.do?page= <--empty
		
		if(page != null && !page.isEmpty()) {
			nowPage = Integer.parseInt(page);
		}
		
		//한 페이지에 표시될 게시물의 시작과 끝번호 계산
		//page가 1이면 1~10까지 계산되야함
		//page가 2이면 11~20까지 계산되야함
		int start = (nowPage-1) * Common.Board.BLOCKLIST + 1;
		int end = start + Common.Board.BLOCKLIST - 1;
		
		HashMap<String, Integer> map = new HashMap<String, Integer>();
		map.put("start",start);
		map.put("end",end);
		
		//페이지 번호에 따른 전체 게시글 조회
		List<BoardVO> list = board_dao.selectList(map);
		
		//전체 게시물 수 조회
		int rowTotal = board_dao.getRowTotal();
		
		//페이지 메뉴 생성하기
		String pageMenu = Paging.getPaging(
				"board_list.do",
				nowPage, //현재 페이지 번호
				rowTotal, //전체 게시물 수
				Common.Board.BLOCKLIST, //한 페이지에 표기할 게시물 수
				Common.Board.BLOCKPAGE); //페이지 메뉴 수
		
		request.getSession().removeAttribute("show");
		
		model.addAttribute("list",list);
		model.addAttribute("pageMenu",pageMenu);
		
		return Common.Board.VIEW_PATH+"board_list/page="+nowPage;		
	}	
}

```


