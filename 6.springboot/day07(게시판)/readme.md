# 게시판 만들기

## Ex_날짜_app 프로젝트 생성

<img width="155" alt="image" src="https://github.com/to7485/Web1500/assets/54658614/d3388ad6-1557-4127-be22-3dd0d49f39ef">

- resoucres 패키지에 config,mapper 패키지 생성하고 파일 복사하기(mapper.xml파일은 하나만 가져와도 된다.), yml파일 복사해서 넣기
- java 패키지에 controller, mapper, service, domain패키지 생성하기 (domain 패키지 안에 dao, dto, vo패키지 만들기)

## config.xml 파일에 alias 지우기
```xml
<?xml version="1.0" encoding="UTF-8"?>
	  
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0/EN" "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

## orderMapper.xml 쿼리문 지우기
```xml
<?xml version="1.0" encoding="UTF-8"?>
	  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0/EN" "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.korea.rest.mapper.OrderMapper">

</mapper>
```





