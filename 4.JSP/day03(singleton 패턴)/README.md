# Singleton Pattern
- jsp 파일을 생성하고 DB에 접속하려고 할 때마다 객체들을 생성하여 메모리를 점유해야 한다.
- 코드를 매번 작성해야 하고 비효율적이다.

## Ex_날짜_Template 프로젝트 생성하기

### 싱글톤
- 객체를 오직 1개만 생성하는 디자인 패턴이다.
- 싱글톤 패턴을 이용하면, 하나의 객체만을 메모리에 등록해서 여러 스레드가 동시에 해당 객체를 공요하여 사용하게끔 할 수 있으므로
- 요청이 많은 곳에서 사용하면 성능상 유리한 이점을 가져올 수 있다.
- DB를 접속할 때 처럼 공통된 객체를 여러개 생성해서 사용해야 하는 상황에 많이 사용된다.

## Singleton Pattern을 설계하는 6가지 방법
1. 가장 기본적인 Singleton(Lazy Initalization : 늦은 초기화)
- 동시성 문제 : 멀티 스레드 환경에서 if문에 두 개이상의 스레드가 동시에 들어가서 실행한다면 객체를 두개이상 만들 수 있음
```java
public class BasicSingleton {
    private static BasicSingleton uniqueInstance;

    public static BasicSingleton getInstance() {
        if(uniqueInstance == null) {
            uniqueInstance = new BasicSingleton();
        }
        return uniqueInstance;
    }
}
```
2. 간단한 Synchronized 적용
- 성능 문제 : 싱글톤을 최초로 생성하는 경우(==null 일때)에만 Lock을 걸면 되는데, 이 코드는 싱글톤 인스턴스를 가져올때 마다 Lock을 건다.
- Thread-safe 보장됨.
```java
public class SynchronizedSingleton {
    private static SynchronizedSingleton uniqueInstance;

    public static synchronized SynchronizedSingleton getInstance() {
        if(uniqueInstance == null){
            uniqueInstance = new SynchronizedSingleton();
        }
        return uniqueInstance;
    }
}
```
3. 클래스 로드 시점에 생성 ( Eager-Initialization : 이른 초기화)
- 성능 문제 : Eager-Initialization 로 인한 문제.
- 클라이언트에서 이 싱글톤을 사용하지 않더라도 이 인스턴스가 로드됨.
```java
public class EagerInitializationSingleton {
    private static EagerInitializationSingleton uniqueInstance = new EagerInitializationSingleton();

    private EagerInitializationSingleton(){

    }

    public static EagerInitializationSingleton getInstance() {
        return uniqueInstance;
    }

}
```
4. Double - Checking - Locking : 동시성 문제와 성능 문제(일부)를 해결(Lazy Initialization : 늦은 초기화)
- 또다른 성능문제: 이 과정에서 하나의 if문 안에 들어갔을 때, synchronized로 locking 하는 방식.
- 이는 캐시 관련 문제를 해결하기 위해 사용하는 키워드인데, volatile을 사용함으로써 또 다른 성능 문제를 가져온다.
- volatile : volatile가 붙은 변수는 CPU의 캐시를 거치지 않고 바로 메인 메모리로 read/write됨.
```java
public  class DoubleCheckingLockingSingleton {
    private volatile static DoubleCheckingLockingSingleton uniqueInstance;

    private DoubleCheckingLockingSingleton(){

    }

    public static DoubleCheckingLockingSingleton getInstance() {
        if(uniqueInstance == null){
            synchronized (DoubleCheckingLockingSingleton.class) {
                if(uniqueInstance == null) {
                    uniqueInstance = new DoubleCheckingLockingSingleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```
5. Enum을 사용한 singleton (Eager-Initialization : 이른 초기화)
- 리플렉션을 통해 싱글톤을 깨트리는 공격에 안전
- 직렬화 보장
- 성능 문제 : Eager-Initialization로 인한 문제와 싱글톤이 Enum 외의 클래스를 상속해야 하는 경우에는 사용할 수 없다.
```java
public enum EnumSingleton {
    uniqueInstance;
}
```

6. LazyHolder 기법 (Lazy Initialization : 늦은 초기화)
- 최근 가장 많이 사용되는 싱글톤 기법으로, 3.클래스 로드 시점에 생성 방법을 개선한 패턴이다.
- Singleton 클래스에는 LazyHolder 클래스의 변수가 없기 때문에 Singleton 클래스 로딩 시 LazyHolder 클래스를 초기화하지 않음
- Class를 로딩하고 초기화하는 시점은 thread-safe를 보장
- holder 안에 선언된 instance가 static이기 때문에 클래스 로딩 시점에 한번만 호출
```java
public class LazyHolderSingleton {

    private LazyHolderSingleton() {
    }

    public static LazyHolderSingleton getInstance() {
        return LazyHolder.uniqueInstance;
    }

    private static class LazyHolder {
        private static final LazyHolderSingleton uniqueInstance = new LazyHolderSingleton();
    }
}
```
### jdbc_templ.xml 등록하기

1. windows -> Preferences

![image](https://user-images.githubusercontent.com/54658614/231664981-f3da563c-173f-45f9-adc8-4584b230f24a.png)

2. java -> Editor -> Templates

![image](https://user-images.githubusercontent.com/54658614/231665073-d25838c1-9ad6-4b0d-b038-a1034aca3172.png)

3. jdbc_templ.xml 열기

![image](https://user-images.githubusercontent.com/54658614/231665119-e13790d4-4f11-4d55-b8ff-916004635340.png)

4. apply and close

![image](https://user-images.githubusercontent.com/54658614/231665175-f74639cf-3ca4-4273-8714-32feee08430c.png)

어차피 DB를 사용하기 위한 코드들은 자바로 이루어져 있기 때문에 DBService 클래스를 만들어서 따로 작성해서 사용을 하자

### DBService 클래스 생성

![image](https://user-images.githubusercontent.com/54658614/231941582-61eeaf53-4f55-42ab-8ad2-58edad8478de.png)

```java
package service;

public class DBService {

  _single 키워드 작성하여 자동완성하기

	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static DBService single = null;

	public static DBService getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new DBService();
		//생성된 객체정보를 반환
		return single;
	}
}

```

![image](https://user-images.githubusercontent.com/54658614/231942441-12d8f7d9-dcfc-4e9d-a079-f01df4f9d223.png)

코드 추가하기

```
package service;

import java.sql.Connection;
import java.sql.SQLException;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import javax.sql.DataSource;

public class DBService {
	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static DBService single = null;

	public static DBService getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new DBService();
		//생성된 객체정보를 반환
		return single;
	}
	
	DataSource ds;
  //ds만 전역으로 만든 이유 생성자 밖에서 사용해야할 일이 있기 때문
	
	//맨처음 if문을 통해 객체 생성이 될 때 딱 1번만 호출되고 이후에는
	//if문으로 들어갈 수 없어 호출될 수 없다.
	public DBService() {
		try {
			InitialContext ic = new InitialContext();
			Context ctx = (Context)ic.lookup("java:comp/env");
			ds = (DataSource)ctx.lookup("jdbc/oracle_test");
			
			
			
		} catch (NamingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	//생성자에서 준비해둔 정보를 기반으로 DB에 연결
	public Connection getConnection() {
		Connection conn = null;
		try {
			conn = ds.getConnection();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return conn;
	}
}

```

DBService 클래스는 DB접속을 위한 명령어를 모아놓은 클래스

쿼리문을 작성하고 데이터를 가져오는 것은 DAO라고 하는 클래스에서 해보자.

### DeptDAO 클래스 생성
- DAO(Data Access Object) : DB접속을 목적으로 하는 클래스

![image](https://user-images.githubusercontent.com/54658614/231943481-640f7863-96b8-4d5b-ad0e-d8a4a7c7d4bc.png)

#### DeptVO 클래스 생성
- DB에서 조회한 내용을 담아줄 그릇이 있어야 하기 때문에 VO를 만들어주자.

![image](https://user-images.githubusercontent.com/54658614/231944307-7219b0c6-a657-4e44-b333-05e08a2fb524.png)

#### DeptVO 클래스 코드 작성하기

```java
package vo;

public class DeptVO {

	private int deptno;
	private String dname,loc;
	
	public int getDeptno() {
		return deptno;
	}
	public void setDeptno(int deptno) {
		this.deptno = deptno;
	}
	public String getDname() {
		return dname;
	}
	public void setDname(String dname) {
		this.dname = dname;
	}
	public String getLoc() {
		return loc;
	}
	public void setLoc(String loc) {
		this.loc = loc;
	}
	
	
}

```
#### DeptDAO 클래스 코드 작성하기

```java
package dao;

public class DeptDAO {
	
	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static DeptDAO single = null;

	public static DeptDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new DeptDAO();
		//생성된 객체정보를 반환
		return single;
	}
	
	//모든 부서를 조회
	public List<argType> selectList() {

		List<argType> list = new ArrayList<argType>();
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		String sql = "select * from dept";

		try {
			//1.Connection얻어온다
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체정보를 얻어오기
			pstmt = conn.prepareStatement(sql);

			//3.결과행 처리객체 얻어오기
			rs = pstmt.executeQuery();

			while (rs.next()) {
				argType vo = new argType();
				//현재레코드값=>Vo저장
				vo.setDeptno(rs.setInt("deptno"));
				vo.setDname(rs.setString("dname"));
				vo.setLoc(rs.setString("loc"));
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
	}
}

```
#### 결과를 보기위한 dept_list.jsp 만들기

```jsp
<%@page import="vo.DeptVO"%>
<%@page import="java.util.List"%>
<%@page import="dao.DeptDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%
	DeptDAO dao = DeptDAO.getInstance();
	List<DeptVO> list = dao.selectList();

%>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
	</head>
	<body>
		<table border="1">
			<caption>부서목록</caption>
			
			<tr>
				<th>부서번호</th>
				<th>부서명</th>
				<th>부서위치</th>
			</tr>
			
			<% for( int i = 0; i < dept_list.size(); i++ ){
				DeptVO dv = dept_list.get(i);%>
				
			<tr>
				<td> <%= dv.getDeptno() %> </td>
				<td> <a href="javascript:send(<%=dv.getDeptno()%>)">
				 	 <%= dv.getDname() %>
				 	 </a>
				</td>
				<td> <%= dv.getLoc() %> </td>
			</tr>
			
			<%} %>
			
		</table>
	</body>
</html>
```
### 순서가 헷갈린다면
1. VO만들기(변수, setter&getter)
2. DAO만들기(싱글톤,조회메서드(\_select),sql문장 및 while문에서 코드 처리)
3. jsp에서 DAO의 결과값을 가져온 후 body영역에 출력하기

### 사원테이블 조회하기

#### SawonVO 클래스 만들기

```java
package vo;

public class SawonVO {

	private int sabun, deptno;
	private String saname, sajob, sahire;
	
	public int getSabun() {
		return sabun;
	}
	public void setSabun(int sabun) {
		this.sabun = sabun;
	}
	public int getDeptno() {
		return deptno;
	}
	public void setDeptno(int deptno) {
		this.deptno = deptno;
	}
	public String getSaname() {
		return saname;
	}
	public void setSaname(String saname) {
		this.saname = saname;
	}
	public String getSajob() {
		return sajob;
	}
	public void setSajob(String sajob) {
		this.sajob = sajob;
	}
	public String getSahire() {
		return sahire;
	}
	public void setSahire(String sahire) {
		this.sahire = sahire;
	}
	
	
}

```

#### SawonDAO 클래스 만들기

```java
package dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import service.DBService;
import vo.SawonVO;

public class SawonDAO {

	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static SawonDAO single = null;

	public static SawonDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new SawonDAO();
		//생성된 객체정보를 반환
		return single;
	}
	
	public List<SawonVO> selectList() {

		List<SawonVO> list = new ArrayList<SawonVO>();
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		String sql = "select * from sawon";

		try {
			//1.Connection얻어온다
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체정보를 얻어오기
			pstmt = conn.prepareStatement(sql);

			//3.결과행 처리객체 얻어오기
			rs = pstmt.executeQuery();

			while (rs.next()) {
				SawonVO vo = new SawonVO();
				//현재레코드값=>Vo저장
				vo.setSaname(rs.getString("saname"));
				vo.setSajob(rs.getString("sajob"));
				
				//String s ="1993-07-25 00:00:00"
				String s = rs.getString("sahire");
				String[] s_date = s.split(" ");
				vo.setSahire(s_date[0]);
				
				vo.setSabun(rs.getInt("sabun"));
				vo.setDeptno(rs.getInt("deptno"));
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
	}
}

```
#### sawon_list.jsp 생성하기

```jsp
<%@page import="java.util.ArrayList"%>
<%@page import="vo.SawonVO"%>
<%@page import="java.util.List"%>
<%@page import="dao.SawonDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%
	SawonDAO dao = SawonDAO.getInstance();
	List<SawonVO> list = new ArrayList<SawonVO>();
%>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
	</head>
	<body>
		<table border="1">
			<tr>
				<th>사번</th>
				<th>이름</th>
				<th>직책</th>
				<th>부서번호</th>
			</tr>
			<%for(int i = 0; i<list.size(); i++){
				SawonVO vo = list.get(i); %>
			<tr>
				<td><%= vo.getSabun()  %></td>
				<td><%= vo.getSaname()  %></td>
				<td><%= vo.getSajob()  %></td>
				<td><%= vo.getDeptno()  %></td>
			</tr>
			<%} %>
		</table>
	</body>
</html>
```




