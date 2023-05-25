# 단위테스트
- 단위테스트는 하나의 모듈을 기준으로 독립적으로 진행되는 가장 작은 단위의 테스트다.
- 하나의 모듈이란 각 계층에서의 하나의 기능 또는 메소드로 이해할 수 있다.
- 하나의 기능이 올바르게 동작하는지를 독립적으로 테스트하는 것이다.

## 단위테스트의 필요성
- 일반적으로 테스트 코드를 작성한다고 하면 거의 단위 테스트를 의미한다.
- 통합 테스트는 실제 여러 컴포넌트들 간의 상호작용을 테스트 하기 때문에 모든 컴포넌트들이 구동된 상태에서 테스트를 하게 되므로, 캐시나 데이터베이스 등 다른 컴포넌트들과 실제 연결을 해야하고 어플리케이션을 구성하는 컴포넌트들이 많아 질수록 테스트를 위한 시간이 커진다.
- 하지만, 단위 테스트는 테스트하고자 하는 부분만 독립적으로 테스트를 하기 때문에 해당 단위를 유지 보수 또는 리팩토링 하더라도 빠르게 문제 여부를 확인 할 수 있다.

## 단위테스트의 한계
- 일반적으로 어플리케이션은 하나의 기능을 처리하기 위해 다른 객체들과 데이터를 주고 받는 복잡한 통신이 일어난다.
- 단위 테스트는 해당 기능에 대한 독립적인 테스트기 때문에 다른 객체와 데이터를 주고 받는 경우에 문제가 발생한다.
- 그래서, 이 문제를 해결하기 위해 테스트하고자 하는 기능과 연관된 모듈에서 가짜 데이터, 정해진 반환값이 필요하다.
- 즉 단위 테스트에서는, 테스트 하고자 하는 기능과 연관된 다른 모듈은 연결이 단절 되어야 비로소 독립적인 단위 테스트가 가능해 진다.

## Ex_날짜_Test 프로젝트 생성
- pom.xml, resources패키지 옮기기

### DB를 톰캣을 통해서 보지 않고 콘솔에서 보기 위해 ojdbc를 직접 넣어주기

![image](https://github.com/to7485/Web1500/assets/54658614/f02b945d-3315-4aa7-ba01-59797e15c7f7)

배포할때 ojdbc 라이브러리도 같이 배포하라는 뜻
![image](https://github.com/to7485/Web1500/assets/54658614/fd335311-e683-4ced-9f90-5ec437f4b61a)

## pom.xml에 spring-test 추가하기

![image](https://github.com/to7485/Web1500/assets/54658614/a3fb357a-615c-41d0-8619-f447b4dd17ed)


```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.1.20.RELEASE</version>
</dependency>
 ```
 
 ## src/test/java의 com.korea.test패키지에 DataSourceTests 클래스 만들기
 
 ```java
 package com.korea.test;

import static org.junit.Assert.fail;

import java.sql.Connection;

import javax.sql.DataSource;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import context.Context_1_dataSource;
import lombok.extern.log4j.Log4j;

//JUnit 프레임워크의 테스트 실행방법을 확장할 때 사용하는 어노테이션
//톰캣 대신에 스프링에 접근할 수 있게 해준다.
@RunWith(SpringJUnit4ClassRunner.class)

//설정 파일을 읽어야 하는데, 이런 설정파일을 로드하는 어노테이션이 ContextConfiguration이다.
@ContextConfiguration(classes={Context_1_dataSource.class})

//로그 객체를 생성합니다.
@Log4j
public class DataSourceTests {

	@Autowired
	private DataSource dataSource;
	
	@Test
	public void testConnection() {
		try(Connection connection = dataSource.getConnection()){
			log.info(connection);
		} catch( Exception e) {
			fail(e.getMessage());
		}
	}
}

 ```
 
 ### 실행했을 때 콘솔에 db접속과 관련된 정보가 출력된다.
 
 ![image](https://github.com/to7485/Web1500/assets/54658614/d6106c67-e369-4378-be1c-561ff66bad2f)

