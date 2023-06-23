# OracleDatabase와 Springboot 연결하기

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

import java.io.IOException;

import javax.sql.DataSource;

import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import lombok.RequiredArgsConstructor;

@Configuration
@RequiredArgsConstructor 
public class MyBatisConfig {
    private final ApplicationContext applicationContext; 

    @ConfigurationProperties(prefix = "spring.datasource.hikari")
    @Bean
    public HikariConfig hikariConfig() {
	    return new HikariConfig();
    }

    @Bean
    public DataSource dataSource() {
	    return new HikariDataSource(hikariConfig());
    }

    @Bean
    public SqlSessionFactory sqlSessionFactory() throws IOException {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource());
        sqlSessionFactoryBean.setMapperLocations(applicationContext.getResources("classpath*:/mapper/*.xml"));
        sqlSessionFactoryBean.setConfigLocation(applicationContext.getResource("classpath:/config/config.xml"));
	//classpath가 resources까지의 경로를 알고 있다.

        try {
            SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBean.getObject();
            sqlSessionFactory.getConfiguration().setMapUnderscoreToCamelCase(true);
            return sqlSessionFactory;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
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

