# 방명록 만들기

## Ex_날짜_visit 프로젝트 만들기
- pom.xml과 resources에 있는 패키지들 가져오기
    - Artifact Id, Name 수정해주기
- web,root,servlet.xml 삭제하기
- mapper이름 visit으로 바꾸고 mybatis-config.xml에 참조하기

## 방명록 테이블 작성하기
```sql
--방명록 DB

--시퀀스
create sequence seq_visit_idx;

create table visit(
  idx NUMBER(3) PRIMARY KEY,
  name VARCHAR2(50), --작성자
  content VARCHAR2(1000), --내용
  pwd VARCHAR2(50), --비번
  ip VARCHAR2(20), --ip
  regdate DATE --작성일
);

--샘플데이터
insert into visit values(
  seq_visit_idx.nextVal,
  '일길동',
  '내가 1빠',
  '1111',
  '192.1.1.1',
  sysdate
);

insert into visit values(
  seq_visit_idx.nextVal,
  '이길동',
  '내가 2빠',
  '1111',
  '192.1.1.1',
  sysdate
);

commit;
```

## vo 패키지에 VisitVO 만들기
```java
package vo;

import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class VisitVO {

	private int index;
	private String name, content, pwd, ip, regdate;
}
```

## dao패키지에 VisitDAO클래스 만들기
```java
package dao;

import org.apache.ibatis.session.SqlSession;

public class VisitDAO {

	SqlSession sqlSession;
	
	public VisitDAO(SqlSession sqlSession) {
		this.sqlSession = sqlSession;
	}
    
    //방명록 전체 조회
	public List<VisitVO> selectList(){
		List<VisitVO> list = sqlSession.selectList("v.visit_list");
		return list;
	}
}

```

## Context_3_dao클래스에 dao 객체 생성하기
```java
package context;

import org.apache.ibatis.session.SqlSession;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import dao.VisitDAO;

@Configuration
public class Context_3_dao {
	

	@Bean
	public VisitDAO visit_daoBean(SqlSession sqlSession) {
		return new VisitDAO(sqlSession);
	}
}

```

## visit.xml에 쿼리문 작성하기
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="v">

	<select id="visit_list" resultType="visit">
		select * from visit order by idx DESC
	</select>
</mapper>
```

## com.korea.visit패키지에 VisitController클래스 만들기
```java
package com.korea.visit;

import org.springframework.stereotype.Controller;

import dao.VisitDAO;

@Controller
public class VisitController {

	VisitDAO visit_dao;
	
	public VisitController(VisitDAO visit_dao) {
		this.visit_dao = visit_dao;
	}
}

```
## ServletContext에 Controller 객체 생성하기
```java
package mvc;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import com.korea.visit.VisitController;

import dao.VisitDAO;

@Configuration
@EnableWebMvc
//@ComponentScan("com.korea.visit")
//어노테이션에도 상속관계가 있다
/*
 *@Component
 *	ㄴ@Controller
 *	ㄴ@Service
 *	ㄴ@Repository 
 * */
//컴포넌트의 자식객체가 들어있으면 사실 Controller가 아니어도 만들어 질 수 있다.
public class ServletContext implements WebMvcConfigurer {

	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		registry.addResourceHandler("/resources/**").addResourceLocations("/resources/");
	}

	
//	  @Bean 
//	  public InternalResourceViewResolver resolver() {
//	  InternalResourceViewResolver resolver = new InternalResourceViewResolver();
//	  resolver.setViewClass(JstlView.class); resolver.setPrefix("/WEB-INF/views/");
//	  resolver.setSuffix(".jsp"); return resolver; }
	
	@Bean
	public VisitController visitController(VisitDAO visit_dao) {
		return new VisitController(visit_dao);
	}
	 
}

```























