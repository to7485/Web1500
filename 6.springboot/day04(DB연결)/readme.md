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

## com.korea.db 아래에 mybatis 패키지 생성고 MybatisConfig클래스 생성하기
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
      jdbc-url: jdbc:oracle;thin:@localhost:1521:XE
      username: hr
      password: hr  
```

## MybatisConfig에서 yml에 작성한 내용을 가져오기
```java
package com.korea.db.mybatis;

import javax.sql.DataSource;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import lombok.RequiredArgsConstructor;

@Configuration
@RequiredArgsConstructor
public class MybatisConfig {

	
	private final ApplicationContext applicationContext;

  //spring.datasource.hikari 가 붙은 모든 설정을 가져와라
	@ConfigurationProperties(prefix = "spring.datasource.hikari")
	@Bean
	public HikariConfig hikariConfig() {
		return new HikariConfig();
	}
	
	@Bean
	public DataSource dataSource() {
		return new HikariDataSource(hikariConfig());
	}
	
}

```




