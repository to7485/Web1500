# 이미지 갤러리 만들어보기
- 지금까지 배운 기술을 활용해 이미지 갤러리를 만들어보자.

## Ex_날짜_PhotoGallery 프로젝트 생성하기
- 필요한 파일,라이브러리 다 가져오기

![image](https://user-images.githubusercontent.com/54658614/233130136-86ccbd68-3d08-4d50-a81f-d6907307a4cb.png)

톰캣의 lib 폴더에 JSTL을 사용하기 위한 라이브러리를 넣어놨지만<br>
오류가 난다면 WEB-INF폴더의 lib에 직접 넣어주자.

### 테이블 생성하기
```
이미지 갤러리

--시퀀스
create sequence seq_photo_idx;

--테이블
create table photo(
	idx NUMBER(3) PRIMARY KEY,
	title VARCHAR2(100),
	filename VARCHAR2(100), --DB에는 실제로 이미지,음악,사진이 저장되지 않는다.
                           이름을 가지고 절대경로에서 가져오는것
	pwd VARCHAR2(50),
	ip VARCHAR2(20),
	regidate DATE
);

업로드부터 해야하기 때문에 샘플데이터는 넣지 않는다.

```

### vo 패키지와 PhotoVO 클래스 생성하기

```java
package vo;

public class PhotoVO {
	private int idx;
	private String title;
	private String filename;
	private String pwd;
	private String ip;
	private String regidate;
	
	
	public int getIdx() {
		return idx;
	}
	public void setIdx(int idx) {
		this.idx = idx;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public String getFilename() {
		return filename;
	}
	public void setFilename(String filename) {
		this.filename = filename;
	}
	public String getPwd() {
		return pwd;
	}
	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
	public String getIp() {
		return ip;
	}
	public void setIp(String ip) {
		this.ip = ip;
	}
	public String getRegidate() {
		return regidate;
	}
	public void setRegidate(String regidate) {
		this.regidate = regidate;
	}
}

```
### dao 패키지와 PhotoDAO 클래스 만들기
- 일단 싱글톤 코드만 작성해놓자.
```java
package dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import service.DBService;
import vo.PhotoVO;

public class PhotoDAO {
	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static PhotoDAO single = null;

	public static PhotoDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new PhotoDAO();
		//생성된 객체정보를 반환
		return single;
	}
}

```
### WebContent에 upload폴더 만들기

![image](https://user-images.githubusercontent.com/54658614/233132337-4fa2b7f1-4e12-4801-bd3d-83dfc509ebc1.png)

### photo_list.jsp 생성하기

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

	<!-- 외부스타일시트 참조 -->
	<link rel="stylesheet" href="css/photo.css">
	
	<script src="js/httpRequest.js"></script>
	
</head>
<body>
	<div id="main_box">
	
		<h1>:::PhotoGallery:::</h1>
		
		<div align = "center" style="margin:10px;">
			<input type="button" value="사진등록">
		</div>
		
		<div id="photo_box">
			<c:forEach begin="1" end="20"> 임시 for문
				<div class="photo_type">
					<img src="">
					<div class="title">제목</div>
					<div align="center">
						<input type="button" value="삭제">
					</div>
				</div>
			</c:forEach>
		</div>
	</div>
</body>
</html>
```
css로 갤러리를 꾸며보자.

### WebContent에 css폴더 만들기

![image](https://user-images.githubusercontent.com/54658614/233133962-db4a53fc-abe9-4d74-8b48-80fefc790b40.png)

- photo.css 만들기

```css
@charset "UTF-8";
*{margin:0; padding:0;}

#main_box h1{ text-align:center;
			  text-shadow:3px 3px 5px gray;
			  color:white; }
			  
#main_box{ width:800px;
		   margin:0 auto; }
		   
#photo_box{ margin:20px auto;
			width:710px;
			height:665px;
			border:1px solid blue;
			padding:10px;
			overflow:auto; /* 스크롤이 필요할때만 보여주는 옵션 */
		    box-shadow:5px 5px 10px gray;}

.photo_type{ width:150px;
			height:200px;
			border:1px solid green;
			float:left;
			margin:10px; }	 	
			
.photo_type img{ display:block;
			 margin:auto;
			 width:98%;
			 height:150px; }	 				
```

![image](https://user-images.githubusercontent.com/54658614/233135594-ea8b81a0-c34b-4d85-99bf-59a5a0436ce5.png)

이제 조회했을 때 데이터가 있는곳만 보이고 없으면 안보이게 바꿔주자

### PhotoDAO에 selectList메서드 추가하기
```java
.....

//이미지 갤러리 전체 조회
	public List<PhotoVO> selectList() {

		List<PhotoVO> list = new ArrayList<PhotoVO>();
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		String sql = "select * from photo order by idx DESC"; 1.sql문 입력하기

		try {
			//1.Connection얻어온다
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체정보를 얻어오기
			pstmt = conn.prepareStatement(sql);

			//3.결과행 처리객체 얻어오기
			rs = pstmt.executeQuery();

			while (rs.next()) {
				PhotoVO vo = new PhotoVO();
				//현재레코드값=>Vo저장
				vo.setIdx(rs.getInt("idx"));
				vo.setTitle(rs.getString("title"));
				vo.setFilename(rs.getString("Filename"));
				vo.setPwd(rs.getString("pwd"));
				vo.setIp(rs.getString("ip"));
				vo.setRegidate(rs.getString("regidate"));

				//ArrayList추가
				list.add(vo);
			}

		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		} finally {

			try {
				if (rs != null)
					rs.close();
				if (pstmt != null)
					pstmt.close();
				if (conn != null)
					conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}

		return list;
	}//select_list
```
앞으로는 DAO에 접근 해서 메서드를 호출을 할 때는 JSP 대신 Servlet이 할 것이다.

### action패키지 만들고 PhotoListAction 생성하기
- jsp로 실행을 하게 되면 리스트가 비어있기 때문에 앞으로는 PhotoListAction에서 실행을 한다.
```java
package action;

import java.io.IOException;
import java.util.List;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.PhotoDAO;
import vo.PhotoVO;


/**
 * Servlet implementation class PhotoListAction
 */
@WebServlet("/list.do")
public class PhotoListAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//DAO로 접근하여 photo전체 목록을 조회
		List<PhotoVO> list = PhotoDAO.getInstance().selectList();
		
		//리퀘스트 스코프 영역에 list객체를 '바인딩'
		request.setAttribute("list", list);
				
		//리퀘스트 영역에 바인딩 되어 list를 어떤 jsp에서 사용할 것인지를 알려준다(포워딩)
		RequestDispatcher disp = request.getRequestDispatcher("photo_list.jsp");
		disp.forward(request, response);
	}

}

```

### photo_list.jsp의 forEach문 수정하기
```jsp
<c:forEach var="vo" items="${list}"> 리스트에 아무것도 없기 때문에 실행해도 아무것도 뜨지 않는게 정상이다.
				<div class="photo_type">
					  <img src="">
					  <div class="title">제목</div>
            <div align="center">
						  <input type="button" value="삭제">
					  </div>
				</div>
			</c:forEach>
```

## 사진등록하기

### photo_list.jsp에 코드 수정하기
```jsp
<h1>:::PhotoGallery:::</h1>
		
		<div align = "center" style="margin:10px;">
			<input type="button" value="사진등록" onclick="location.href='insert_form.jsp'">
      프로젝트의 확장성을 염두해둔다면 Servlet으로 보내도 무방하다.
		</div>
```

### insert_form.jsp 생성하고 코드 작성하기
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	function send(f) {
		var title =f.title.value;
		var pwd = f.pwd.value;
		var photo = f.photo.value; 사진파일 그 자체
		
		if(title=='') {
			alert("제목을 입력해야 합니다");
			return;
		}
		
		f.submit();//insert.do로 파라미터를 가지고 화면을 전환
		
	}
</script>
</head>
<body> 
	<form action="insert.do" method="post" enctype="multipart/form-data">
			
			<table border="1" align="center">
				<caption>사진 등록하기</caption>
				<tr>
					<th>제목</th>
					<td><input name="title"></td>
				</tr>
				
				<tr>
					<th>비밀번호</th>
					<td><input name="pwd" type="password"></td>
				</tr>
				<tr>
					<th>등록할 사진</th>
					<td><input name="photo" type="file"></td>
				</tr>
				
				<tr>
					
					<td colspan="2" align="center">
					<input type="button" value="등록하기" onclick="send(this.form);">
					<input type="button" value="목록으로" onclick="location.href='list.do'">
					jsp로 바로 돌아가는것이 아니라 db를 한번 거치고 가야 하기 때문에 Servlet으로 가야한다.
					</td>
				</tr>
			</table>
	
	</form>
</body>
</html>
```

### PhotoInsertAction 서블릿 만들기

```java
package action;

import java.io.File;
import java.io.IOException;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.oreilly.servlet.MultipartRequest;
import com.oreilly.servlet.multipart.DefaultFileRenamePolicy;

import dao.PhotoDAO;
import vo.PhotoVO;

/**
 * Servlet implementation class PhotoInsertAction
 */
@WebServlet("/insert.do")
public class PhotoInsertAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// insert.do?title=aaa&pwd=11111&photo=^&^&^&
		String web_path = "/upload/";
		ServletContext application = request.getServletContext();
		String path = application.getRealPath(web_path);
		System.out.println(path);
		
		int max_size = 1024 * 1024 * 100; //최대 업로드 용량 100mb
		MultipartRequest mr = new MultipartRequest(request, path, max_size, "utf-8", new DefaultFileRenamePolicy());
    여기까지 작성하면 사진이 절대경로에 업로드는 된다.
		
		String filename="";
		File f = mr.getFile("photo");
		if( f != null) {
			filename = f.getName(); //업로드된 파일의 실제 파일명	
		}
		
		//파일형식 이외의 나머지 파라미터 수신
		String title = mr.getParameter("title");
		String pwd = mr.getParameter("pwd");
		String ip = request.getRemoteAddr();
		
		PhotoVO vo = new PhotoVO();
		vo.setTitle(title);
		vo.setFilename(filename);
		vo.setPwd(pwd);
		vo.setIp(ip);
		
		int res = PhotoDAO.getInstance().insert(vo);
		
		if( res >= 1) {
			response.sendRedirect("list.do");
		}
				
	}
}
```

### PhotoDAO에 insert메서드 추가하기
```java
...

//사진등록
	public int insert(PhotoVO vo) {
		// TODO Auto-generated method stub
		int res = 0;

		Connection conn = null;
		PreparedStatement pstmt = null;

		String sql = "insert into photo values(seq_photo_idx.nextval, ?, ?, ?, ?, sysdate)";

		try {
			//1.Connection획득
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체 획득
			pstmt = conn.prepareStatement(sql);

			//3.pstmt parameter 채우기
			pstmt.setString(1, vo.getTitle());
			pstmt.setString(2, vo.getFilename());
			pstmt.setString(3, vo.getPwd());
			pstmt.setString(4, vo.getIp());
			//4.DB로 전송(res:처리된행수)
			res = pstmt.executeUpdate();

		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		} finally {

			try {
				if (pstmt != null)
					pstmt.close();
				if (conn != null)
					conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		return res;
	}//insert
```
사진등록을 해서 하나가 나오는걸 확인해보자.

![image](https://user-images.githubusercontent.com/54658614/233149201-59df0f09-cd7b-4151-8d45-9616b1d54c1d.png)

### photo_list.jsp에 데이터 집어넣어주기

```jsp
<c:forEach var="vo" items="${list}"> 리스트에 아무것도 없기 때문에 실행해도 아무것도 뜨지 않는게 정상이다.
				<div class="photo_type">
					  <img src="upload/${vo.filename}">
					  <div class="title">${vo.title}</div>
            <div align="center">
						  <input type="button" value="삭제">
					  </div>
				</div>
			</c:forEach>
```

삭제를 하기 위해 비밀번호를 입력할 공간을 만들어주자

```jsp
<script>
	function del(f){
		var idx = f.idx.value;
		var pwd = f.pwd.value; //원본비밀번호
		var pwd2 = f.pwd2.value; //입력한 비밀번호

		alert(f.idx.value);

		if(pwd != pwd2) {
			alert("비밀번호가 일치하지 않습니다");
			return;
		}

		if(!confirm("삭제?")){
			return;
		}
		//삭제를 원하는 idx를 서버로 전송
		var url = "photo_del.do";
//encodeURIComponent : 전달할 파라미터에 특수문자가 섞여있다면 오류를 방지하기 위해 사용해주는 인코딩 메서드
		var param = "idx="+encodeURIComponent(idx)+"&filename="+f.filename.value;

		sendRequest(url, param, finRes,"POST");
	}

	function finRes(){
		if(xhr.readyState == 4 && xhr.status ==200){

			//서블릿으로부터 도착한 데이터 읽어오기
			var data = xhr.responseText;

			//넘겨받은 data는 ""로 묶여진 문자열 구조로 인식하기 때문에
			//JSON형식으로 변경을 해줘야 한다.
			//data ---> "[{'param':'no'}]"
			var json = eval(data);

			if(json[0].param == 'yes') {
				alert("삭제성공");	//Ajax를 사용하지 않으면 삭제 되고 나서 경고창을 띄울수 없다.
			}else {
				alert("삭제실패");
			}

			location.href="list.do";
		}
	}
</script>


......

<c:forEach var="vo" items="${list}"> ${vo.idx}
	<div class="photo_type">
		<img src="upload/${vo.filename}">

		<div class="title">${vo.title}</div>

	<form>
		<input type="hidden" name="idx" value="${vo.idx}"> 지우기 위해 index번호를 보내주자.
		<input type="hidden" name="pwd" value="${vo.pwd}"> 원본비밀번호(내가 입력한 비밀번호와 일치하면 삭제 하게 해주자)
		<input type="hidden" name="filename" value="${vo.filename}">

		<div align="center">
			<input type="password" name="pwd2" size="5">
			<input type="button" value="삭제" onclick="del(this.form)">
		</div>
	</form>
	</div>
	</c:forEach>
```

### PhotoDelAction 서블릿 만들기
```java
package action;

import java.io.File;
import java.io.IOException;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.PhotoDAO;

/**
 * Servlet implementation class PhotoDelAction
 */
@WebServlet("/photo_del.do")
public class PhotoDelAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		
		//photo_del.do?idx=3&filename=aaaaa
		request.setCharacterEncoding("utf-8");
		
		int idx = Integer.parseInt(request.getParameter("idx"));
		String filename = request.getParameter("filename");
		
		String web_path = "/upload/";
		ServletContext app = request.getServletContext();
		String path = app.getRealPath(web_path);
		
		//idx에 해당되는 글 삭제
		int res = PhotoDAO.getInstance().delete(idx);
		
		if(res > 0) {
			File f = new File(path, filename); //파일클래스가 경로로 접근해서 파일이름을 찾는다.
			if(f.exists()) {
				f.delete(); //path경로의 파일 제거
			}
		}
		
		String param = "no";
		if(res > 0) {
			param = "yes";
		}
		
		//결과값인 param을 json 구조로 포장
		String resultStr = String.format("[{'param':'%s'}]", param);
		
		//콜백메서드로 복귀
		response.getWriter().print(resultStr);
	}

}
```

### PhotoDAO에 삭제 메서드 추가하기
```java
public int delete(int idx) {
		// TODO Auto-generated method stub
		int res = 0;

		Connection conn = null;
		PreparedStatement pstmt = null;

		String sql = "delete from photo where idx=?";

		try {
			//1.Connection획득
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체 획득
			pstmt = conn.prepareStatement(sql);

			//3.pstmt parameter 채우기
			pstmt.setInt(1, idx);
			//4.DB로 전송(res:처리된행수)
			res = pstmt.executeUpdate();
			System.out.println("res: " + res);
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		} finally {

			try {
				if (pstmt != null)
					pstmt.close();
				if (conn != null)
					conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		return res;
	}
```

# 이미지 다운로드 받기

## util패키지를 만든후 FileDownload.java 서블릿파일 만들기

```java
package util;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.net.URLEncoder;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * IE 8.0인경우 화일다운로드 요청을 2회실시
 * (이유는 정확이 모르겠지만 화일다운로드 다이아로그가 띄어지면서 다시 호출하는것 같음.)
 * 첫번째 요청인경우 한글인코딩이 제대로 이뤄지는데
 * 두번째는 한글이 깨진다
 * 그래서 첫번째값만 저장해놓고 그값을 사용한다.
 */

@WebServlet("/FileDownload.do")
public class FileDownload extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//request.setCharacterEncoding("utf-8");
		String dir = request.getParameter("dir"); //파라미터로 받아온 경로
		String fullpath = getServletContext().getRealPath(dir); //그 경로의 절대경로
		String filename = "";
		filename = request.getParameter("filename"); //파라미터로 넘긴 실제 파일 이름
		String fullpathname = String.format("%s/%s", fullpath,filename); //경로/파일이름
		//System.out.println(fullpathname);
		File file = new File(fullpathname); //파일클래스의 생성자에 넣기
		byte [] b = new byte[1024*1024*4];
		
	// 사용자 브라우저 타입 얻어오기
        String strAgent = request.getHeader("User-Agent");
        String userCharset = request.getCharacterEncoding();
        if(userCharset==null)userCharset="utf-8";
        
        //System.out.println("filename:"+filename+"\nagent:"+strAgent+"\ncharset:"+userCharset);
        //System.out.println("----------------------------------------------------------------");
        String value = "";
        // IE 일 경우
        if (strAgent.indexOf("MSIE") > -1) 
        {
             // IE 5.5 일 경우
            if (strAgent.indexOf("MSIE 5.5") > -1) 
            {
                value = "filename=" + filename ;
            }
             // 그밖에
            else if (strAgent.indexOf("MSIE 7.0") > -1) 
            {
                if ( userCharset.equalsIgnoreCase("UTF-8") ) 
                {
                	filename = URLEncoder.encode(filename,userCharset);
                	filename = filename.replaceAll("\\+", " ");
                    value = "attachment; filename=\"" + filename + "\"";

                }    
                else 
                {
                    value = "attachment; filename=" + new String(filename.getBytes(userCharset), "ISO-8859-1");
                   
                }
            }
            else{
            	//IE 8.0이상에서는 2회 호출됨..
            	if ( userCharset.equalsIgnoreCase("UTF-8") ) 
                {
                	filename = URLEncoder.encode(filename,"utf-8");
                	filename = filename.replaceAll("\\+", " ");
                    value = "attachment; filename=\"" + filename + "\"";
            		
                }    
                else 
                {
                    value = "attachment; filename=" + new String(filename.getBytes(userCharset), "ISO-8859-1");
                   
                }
            }
            
            
        }else if(strAgent.indexOf("Firefox") > -1){
        	//Firefox : 공백문자이후은 인식안됨...
        	value = "attachment; filename=" + new String(filename.getBytes(), "ISO-8859-1");
        }
       else {
            // IE 를 제외한 브라우저
            value = "attachment; filename=" + new String(filename.getBytes(), "ISO-8859-1");
        }
        
   
        response.setContentType("Pragma: no-cache"); 

	//전송 데이터가 stream 처리되도록 : 웹상전송 문자셋은 : 8859_1
	response.setContentType("application/octet-stream;charset=8859_1;");

	//데이터 형식 성향설정 (attachment : 첨부파일)
	//Content-Disposition : attachment
	 response.setHeader("Content-Disposition", value);

	//내용물 인코딩방식 결정 - 전송타입은 binary(이진파일)
	response.setHeader("Content-Transfer-Encoding", "binary;");

	if(file.isFile())
		{
			//FileInputStream의 경우 자신이 객체를 생성하여 입력을 시도할 수 있다.
			//하지만 BufferedStream 객체는 미리 만들어진 Stream의 생성자에 인자로 받아서 객체를 생성한다.
			BufferedInputStream bis = new BufferedInputStream(new FileInputStream(file));
			BufferedOutputStream bos = new BufferedOutputStream(response.getOutputStream());
			int i=0;
			try
			{
				while((i=bis.read(b))!=-1)
				{
					bos.write(b,0,i);
				}
			}catch(Exception e){
				//e.printStackTrace();
			}finally {
				if(bos!=null)bos.close();
				if(bis!=null)bis.close();

			}
		}
	}
	
}

```

## photo_list.jsp 코드 추가하기
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	
	......................
	
<script type="text/javascript">

		function del(f){
			...........	
		}
		
		function resultFn(){
			...........
		}//resultFn()
		
		var click = false;
		
		function imgclick(idx){
			...........
		}//imgclick()
		
		function download( filename ){
			
	//파일다운로드 서블릿 호출( upload폴더에 저장되어 있는 filename을 서블릿으로 전달 )
			location.href="../download.do?dir=/upload/&filename="+
					encodeURIComponent(filename);
				
		}//download()
		
	</script>

</head>

<body>

	<div id="main_box">

		<h1>:::PhotoGallery:::</h1>

		<div align="center" style="margin: 10px">
			<input type="button" value="사진올리기"
				onclick="location.href='insert_form.do'">
		</div>

		<div id="photo_box">

			...............

			<c:forEach var="vo" items="${list}">
				<div class="photo_type">
				
					<img ............>
					<div class="title">${vo.title}</div>

					<form>
					    <input type="hidden" name="idx" value="${vo.idx}">
					    <input type="hidden" name="pwd" value="${vo.pwd}">
						
						<div align="center">
						                                         //수정
						   <input type="password" name="pwd2" size="5">

						   <input type="button" value="삭제" 
									     onclick="del(this.form);">
							
						   //추가
						   <input type="button" value="down" 
							      onclick="download('${vo.filename}');">
						</div>			
					</form>	
				</div>
			</c:forEach>
		</div>
	</div>
	
	<div id="showdiv" style="display:none">
		<img src="" id="showImg" onclick="imgclick(this.src);">
	</div>
	
</body>

</html>
```
