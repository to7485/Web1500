# Ex_날짜_DB 프로젝트 생성하기

## JDBC로 이클립스와 오라클DB 연동하기
- JDBC(Java DataBase Connectivity)는 자바/JSP 프로그램 내에서 데이터베이스와 관련된 작업을 처리할 수 있도록 도와주는 자바 표준 인터페이스로, 관계형 데이터베이스 시스템에 접근하여 SQL문을 실행하기 위한 자바 API 또는 자바 라이브러리입니다. JDBC API를 사용하면 DBMS 종류에 상관없이 데이터베이스 작업을 처리할 수 있습니다.
- JDBC API는 java.sql.\* 패키지에 의해 구현됩니다.
- 프로젝트를 생성한 후 WebContent의 META-INF에 context.xml(실행시 최초로 호출되는 설정파일)을, WEB-INF lib폴더에 준비된 4개의 라이브러리를 넣는다.

![image](https://user-images.githubusercontent.com/54658614/231060513-bec2d76e-3f09-4ccc-844c-5755e70b7e49.png)

- commons-collections-3.1.jar 
  - Java Collection Framework을 상속 확장

- commons-dbcp-1.2.1.jar 
  - Database connection pooling 서비스 제공  데이타소스를 org.apache.commons.dbcp.BasicDataSource를 사용

- commons-pool-1.4.jar
  - 데이터베이스와 연결된 커넥션을 미리 만들어 풀(pool)에 저장해 두었다가 필요할 때에 가져다 쓰고 반환하는 기법이다. <br>아파치에서 제공하는 DBCP (DataBase Connection Pool) API를 이용해서 사용할 수 있다.

### context.xml

![image](https://user-images.githubusercontent.com/54658614/231063002-1c32db00-19f9-4395-b233-e03dda8a5d43.png)

```
<?xml version="1.0" encoding="UTF-8"?>

리소스를 미리 준비해두고 DB에 연결해야 하는 상황에
찾아서 사용하는 구조를 JNDI(Java Naming Directory Interface)

<Context>
	<Resource DB로 접속하기 위한 설정들에 대한 정보
	        auth="Container" 
      		name="jdbc/oracle_test"
      		type="javax.sql.DataSource"
      		driverClassName="oracle.jdbc.driver.OracleDriver"
      		factory="org.apache.commons.dbcp.BasicDataSourceFactory"
      		url="jdbc:oracle:thin:@localhost:1521:xe"
      		username="test" password="1111" -> 어떤 계정으로 로그인을 할건지 
      		maxActive="20" maxIdle="10" maxWait="1"/>
          
          maxActive(최대연결수) : 현재 프로젝트에서 오라클 DB에 연결하는 시간이 1초라고 가정한다면 10명이 동시에 접속을 시도할 경우 10번째 사람은 10초
          뒤에 DB로 접속할 수 있다. 따라서 미리 10명이 동시에 접속할 수 있도록 준비를 해 두면 대기시간 없이 DB에 효율적으로 접근할 수 있다.
          이를 DBCP(Database Connection Pool)이라고 한다.
          
          maxWait : 1초동안만 기다림, 1초 지나면 포기 -1은 무한대기
          
</Context>










```
