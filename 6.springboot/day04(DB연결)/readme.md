![image](https://github.com/to7485/Web1500/assets/54658614/8b275b39-c0a1-4153-9769-ab695c2f898c)# OracleDatabase와 Springboot 연결하기

## Ex_날짜_DB 프로젝트 생성하기
- 기존에는 MvnRepository 사이트를 통해서 필요한 라이브러리들을 일일히 찾아서 다운로드를 받았다면 스프링부트에서는 손쉽게 의존성 추가가 가능하다.

![image](https://github.com/to7485/Web1500/assets/54658614/89c7d872-00b3-4b66-a98e-15a77356c4d3)

![image](https://github.com/to7485/Web1500/assets/54658614/68f6ae7b-abcd-456a-8b71-ab8574022d17)

- application.yml 파일 생성하기
  - 포트번호 바꿔주기

 ![image](https://github.com/to7485/Web1500/assets/54658614/33b5b7e3-8c9f-48a8-9f86-a84b487edbce)

- 스프링을 사용할 때 DataSource에 대한 내용을 설정파일을 따로 만들어서 관리를 해왔다.
- 스프링부트에서도 마찬가지로 사용하면 된다.

## com.korea.db 아래에 mybatis 패키지 생성하고 MybatisConfig클래스 생성하기
```java
package com.korea.db.mybatis;

import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import lombok.RequiredArgsConstructor;

@Configuration //설정파일임을 명시하는 어노테이션
@RequiredArgsConstructor //생성자주입
public class MybatisConfig {

	private final ApplicationContext applicationContext; 

	@Bean
	public HikariConfig hikariConfig() {
		return new HikariConfig();
	}
	
	//유저네임이라던가 비밀번호에 대한 설정을 HikariConfig객체에 넣어줘야 한다.
	//application.yml 파일에 작성을 한다.
	
	@Bean
	public DataSource dataSource() {
	    return new HikariDataSource(hikariConfig());
	}
	
}

```
## 서버와 DB의 연결
- WAS(Web Application Server)와 데이터베이스 사이의 연결에는 많은 자원이 든다.
- 서버가 DB에 연결하기 위한 Connection 자원이 가장 큰 비율을 차지한다.
- 이를 보완할 수 있는 방법이 ConnectionPool이다.

### 커넥션 풀(Connection Pool)이란?
- 데이터베이스와 연결된 커넥션을 미리 만들어놓고 이를 pool로 관리하는것이다.
- 필요할때마다 Connection Pool의 Connection을 이용하고 반환하는 기법이다.
- 미리 만들어놓은 Connection을 이용하면 Connection에 필요한 자원을 줄일수 있다.
- 커넥션 풀을 사용하면 커넥션 수를 제한할 수 있어서 과도한 접속으로 인한 서버 자원 고갈을 방지할 수 있다.
- DB 접속 모듈을 공통화해 DB 서버의 환경이 바뀔 경우 유지보수를 쉽게 할 수 있다.

### HikariCP란?
- HikariCP는 가벼운 용량과 빠른 속도를 가지는 JDBC의 커넥션 풀 프레임워크이다.
- SpringBoot는 커넥션 풀 관리를 위해 HikariCP를 사용한다.
- org.springframework.boot:spring-boot-starter-jdbc에 포함되어있다.

![image](https://github.com/to7485/Web1500/assets/54658614/1d85546f-efa7-4ebf-9f80-455a6715776a)


- Thread가 커넥션을 요청했을 때 유휴 커넥션이 존재한다면 해당 커넥션을 반환해준다.

![image](https://github.com/to7485/Web1500/assets/54658614/188f7309-b889-4a45-9ac5-147e233aa2a3)

### Connection Pool의 크기와 성능
-  Connection을 사용하는 주체인 Thread의 개수보다 커넥션 풀의 크기가 크다면 사용되지 않고 남는 커넥션이 생겨 메모리의 낭비가 발생하게 된다.
-  MySQL의 공식레퍼런스에서는 600여 명의 유저를 대응하는데 15~20개의 커넥션 풀만으로도 충분하다고 언급하고 있다.

우아한 형제들 테크 블로그에서는 다음과 같은 공식을 추천하고 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/c2b28eba-a26d-4f1d-9a4b-3a25666342cd)

[커넥션 풀 size 공식 출처](https://techblog .woowahan.com/2663/)

- Tn = 전체 Thread의 개수
- Cm = 하나의 Task에서 동시에 필요한 Connection 수

- DB와 WAS의 Context Switching 역시 한계가 있기 때문에 Thread Pool의 크기는 Conncetion Pool의 크기를 결정하는데 매우 중요하다.

## application.yml에 코드 작성하기
```yml
server:
  port: 10000

#JDBC datasource
spring:
  datasource:
    hikari:
      driver-class-name: oracle.jdbc.OracleDriver
      jdbc-url: jdbc:oracle:thin:@localhost:1521:XE
      username: hr
      password: hr
```

## MybatisConfig에서 yml에 작성한 내용을 가져오기
```java
package com.korea.db.mybatis;

import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;

import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import lombok.RequiredArgsConstructor;

@Configuration
@RequiredArgsConstructor
public class MybatisConfig {
	
	//HikariCP
	//가벼운 용량과 빠른 속도를 가지는 JDBC의 커넥션풀 프레임워크이다.
	//BOOT는 커넥션 풀 관리를 위해 HikariCP를 사용한다.
	//
	
	@ConfigurationProperties(prefix="spring.datasource.hikari")
	@Bean
	public HikariConfig hikariConfig() {
		return new HikariConfig();
	}
	
	@Bean
	public HikariDataSource dataSource() {
		return new HikariDataSource(hikariConfig());
	}
	
	@Bean
	public SqlSessionFactory sqlSessionFactory() throws Exception{
		SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
		sqlSessionFactoryBean.setDataSource(dataSource());
		sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath*:/mapper/*.xml"));
		sqlSessionFactoryBean.setConfigLocation(new ClassPathResource("config/config.xml"));
		
		SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBean.getObject();
		sqlSessionFactory.getConfiguration().setMapUnderscoreToCamelCase(true);
		return sqlSessionFactory;
	}

}
```

## resources 패키지 아래 mapper,config 폴더 만들기

![image](https://github.com/to7485/Web1500/assets/54658614/782efd6b-72f3-470f-9c05-08cf396a8c55)

- config폴더 아래 config.xml파일 만들고 코드 작성하기
```xml
<?xml version="1.0" encoding="UTF-8"?>
	  
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0/EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
</configuration>
```
- mapper폴더 아래 timeMapper.xml 파일 작성하기
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="">

</mapper>
```

## main/java 아래에 mapper 패키지 만들고 TimeMapper인터페이스 생성하기
- Mapper 설정파일(xml)에 있는 SQL 쿼리문을 호출하기 위한 인터페이스를 생성한다.
- DAO 대신 @Mapper 어노테이션을 사용한다(mybatis 3.0이상)
- @Mapper 어노테이션을 사용하면 Bean으로 등록되며 Service단에서 @Autowired하여 사용할 수 있다.
- 메서드명은 Mapper xml파일의 id와 맞춰야 한다.
- @Mapper가 아닌 @MapperScan 어노테이션을 사용해도 된다.
```java
package com.korea.db.mapper;

import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface TimeMapper {

	public String getTime(); //쿼리문과 1대1로 매치가 됨
}

```

## timeMapper.xml에 쿼리문 작성하기
```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.korea.db.mapper.TimeMapper">
    <select id="getTime" resultType="string">
        SELECT SYSDATE FROM DUAL
    </select>
</mapper>
```

## 단위테스트를 해보자.
- test/java에 mapper 패키지 만들기

![image](https://github.com/to7485/Web1500/assets/54658614/c56baf4a-6770-4697-9663-53f870540daa)

### MapperTest클래스 생성하기
```java
package com.example.db.mapper;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;


import lombok.extern.slf4j.Slf4j;

@SpringBootTest
@Slf4j
public class MapperTests {
    @Autowired private TimeMapper timeMapper;


    @Test
    public void getTimeTest(){
        log.info("timeLog : " + timeMapper.getTime());
    }

   
}
```

![image](https://github.com/to7485/Web1500/assets/54658614/c3bb2626-453e-467f-9563-1a471a1cdcc0)

# 테이블 만들어서 CRUD 해보기

## 상품 테이블 생성하기
```sql
CREATE SEQUENCE SEQ_PRODUCT;

CREATE TABLE PRODUCT(
	PRODUCT_ID NUMBER PRIMARY KEY,
	PRODUCT_NAME VARCHAR2(500) NOT NULL,
	PRODUCT_STOCK NUMBER DEFAULT 0,
	PRODUCT_PRICE NUMBER DEFAULT 0,
	REGISTER_DATE DATE DEFAULT SYSDATE,
	UPDATE_DATE DATE DEFAULT SYSDATE
);
```

## main/java/vo 패키지 생성하고 ProductVO 클래스 생성하기

```java
package com.example.db.vo;

import lombok.Data;

@Data
public class ProductVO {

	private int productId;
	private String productName;
	private int productStock;
	private int productPrice;
	private String registerDate;
	private String updateDate;
}
```

## main/java/mapper 패키지에 ProductMapper.java 인터페이스 생성하기
```java
package com.example.db.mapper;

import org.apache.ibatis.annotations.Mapper;

import com.example.db.vo.ProductVO;

@Mapper
public interface ProductMapper {

	//상품 추가
	public void insert(ProductVO productVO);
}
```

## main/resources에 product.xml 파일 생성하기
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.db.mapper.ProductMapper">
	<insert id="insert">
		insert into product
		(PRODUCT_ID, PRODUCT_NAME, PRODUCT_STOCK, PRODUCT_PRICE)
		values (seq_product.nextVal, #{productName}, #{productStock}, #{productPrice})
	</insert>
</mapper>
```

## DBController 클래스 생성하기

```java
package com.korea.db.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.korea.db.mapper.ProductMapper;
import com.korea.db.mapper.TimeMapper;
import com.korea.db.vo.ProductVO;

import lombok.RequiredArgsConstructor;

@Controller
@RequiredArgsConstructor
public class DBController {
	
	private final ProductMapper productMapper;

	//@RequestMapping(value="register", method={RequestMethod.GET,RequestMethod.POST})
    	//RequestMapping은 GET,POST 둘다 사용이 가능한것이다.

	//@RequestMapping(value="register", method=RequestMethod.POST)	
    @GetMapping("register")
    public String register(Model model){
        model.addAttribute("productVO", new ProductVO());
        return "product-insert";
    }
}

```

## product-insert.html 파일 생성하기
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>상품 추가</title>
</head>
<body>
    <form th:action="@{/register}" th:object="${productVO}" method="post">
        <div>
            <input type="text" th:field="*{productName}" placeholder="상품 이름">
        </div>
        <div>
            <input type="text" th:field="*{productStock}" placeholder="상품 재고">
        </div>
        <div>
            <input type="text" th:field="*{productPrice}" placeholder="상품 가격">
        </div>
        <input type="submit" value="등록">
    </form>
</body>
</html>
```

## DBController에 코드 추가하기
```java
package com.korea.db.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.view.RedirectView;

import com.korea.db.mapper.ProductMapper;
import com.korea.db.mapper.TimeMapper;
import com.korea.db.vo.ProductVO;

import lombok.RequiredArgsConstructor;

@Controller
@RequiredArgsConstructor
public class DBController {
	
	private final ProductMapper productMapper;
	
	@RequestMapping("register")
	public String ex02(Model model) {
		model.addAttribute("vo",new ProductVO());
		
		return "product-insert";
	}
	
    @PostMapping("register")
    public RedirectView register(ProductVO productVO){
        productMapper.insert(productVO);
        return new RedirectView("list");
    }

    @GetMapping("list")
    public String list(Model model){
        return "product-list";
    }
}
```

## 조회하기 메서드 작성하기
```java
package com.korea.db.mapper;

import org.apache.ibatis.annotations.Mapper;

import com.korea.db.vo.ProductVO;

@Mapper
public interface ProductMapper {

	//상품 추가
	public void insert(ProductVO productVO);
	//상품 전체 목록
    	public List<ProductVO> selectAll();
}
```

## product.xml에 쿼리문 작성하기
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.korea.db.mapper.ProductMapper">
	<insert id="insert">
		insert into product
		(PRODUCT_ID, PRODUCT_NAME, PRODUCT_STOCK, PRODUCT_PRICE)
		values (seq_product.nextVal, #{productName}, #{productStock}, #{productPrice})
	</insert>
	
	<select id="selectAll">
        	SELECT PRODUCT_ID, PRODUCT_NAME, PRODUCT_STOCK, PRODUCT_PRICE, REGISTER_DATE, UPDATE_DATE FROM PRODUCT
    	</select>
</mapper>
```

## DBController에 코드 작성하기
```java

... 중략

@GetMapping("list")
public String list(Model model) {
	List<ProductVO> list = productMapper.selectAll();
	model.addAttribute("list",list);
	return "product-list";
}

```

## product-list.html 생성하기
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>상품 목록</title>
</head>
<body>
    <table border="1">
        <tr>
            <th>상품 번호</th>
            <th>상품 이름</th>
            <th>상품 재고</th>
            <th>상품 가격</th>
            <th>등록 날짜</th>
            <th>수정 날짜</th>
        </tr>
        <th:block th:each="product : ${list}">
            <tr th:object="${product}">
                <td th:text="*{productId}"></td>
                <td th:text="*{productName}"></td>
                <td th:text="*{productStock}"></td>
                <td th:text="*{productPrice}"></td>
                <td th:text="*{registerDate}"></td>
                <td th:text="*{updateDate}"></td>
            </tr>
        </th:block>
    </table>
</body>
</html>
```

![image](https://github.com/to7485/Web1500/assets/54658614/17077c9a-41b8-4c1f-a7db-e10df781c0d2)
