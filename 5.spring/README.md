# 프레임워크란?
- Mybatis와 같이 뭔가를 만들기 위한 하나의 기본적인 틀을 의미한다.<br>이곳에 필요한 내용을 조립해서 쓰는 구조라고 생각하면 된다.

스프링 시스템 이전에 EJB(엔터프라이즈 자바빈즈)라는 프레임워크를 사용하고 있었는데, 이는 개발비용이 수억에서 수십억에 달한다.<br>
그러나 Spring을 통해 EJB의 90% 이상의 기능을 구현할 수 있으며, 오픈소스 기반이기 때문에 제작비용이 들지 않는다는 장점이 있다.<br>
이게 스프링을 많이 사용하는 이유다.<br>

전자정부표준프레임워크 = SpringFramework라고 생각해도 무방할 정도로 국내에 입찰되는 정부사업은 대부분 Spring을 사용한다고 보면 된다.<br>

Spring은 아래 그림과 같이 기본 틀을 제공하고 필요한 기능들을 추가해서 사용하는 구조라고 생각하면 된다.<br>
( 로그인, 게시판 등 만들어져 있는 기능을 끼워넣어서 사용하면 되는데, 물론 원하는 모양으로 디자인 하려면 커스터마이징이 필요하긴 하다.)<br>

![image](https://user-images.githubusercontent.com/54658614/236684904-d4e2e78e-245c-421d-912f-ec0ffe9f4e48.png)


# 스프링 프레임워크란?
스프링(Spring)은 매우 방대한 기능을 제공하고 있어서 스프링을 한마디로 정의하기는 힘들다. 흔히 스프링이라고 하면 스프링 프레임워크를 말하는데, 스프링 프레임워크의 주요 특징은 다음과 같다.
- <b>의존 주입(Dependency Inject : DI)</b> 지원
- <b>AOP(Aspect-Oriented Programming)</b> 지원
- <b>MVC 웹 프레임워크</b> 제공
- <b>JDBC, JPA 연동, 선언적 트랜잭션 처리 등 DB 연동</b> 지원<br><br>

이 외에도 스케줄링, 메시지 연동(JMS), 이메일 발송, 테스트 지원 등 자바 기반 어플리케이션을 개발하는데 필요한 다양한 기능을 제공한다.<br><br>

실제로 스프링 프레임워크를 이용해서 웹 어플리케이션을 개발할 때에는 스프링 프레임워크만 단독으로 사용하기 보다는 여러 스프링 관련 프로젝트를 함께 사용한다. 현재 스프링을 주도적으로 개발하고 있는 피보탈(Pivotal)은 스프링 프레임워크뿐만 아니라 어플리케이션 개발에 필요한 다양한 프로젝트를 진행하고 있다. 이들 프로젝트 중 자주 사용되는 것은 다음과 같다.
- <b>스프링 데이터</b> : 적은 양의 코드로 데이터 연동을 처리할 수 있도록 도와주는 프레임워크이다. JPA, 몽고DB, 레디스 등 다양한 저장소 기술을 지원한다.
- <b>스프링 시큐리티</b> : 인증/인가와 관련된 프레임워크로서 웹 접근 제어, 객체 접근 제어, DB - 오픈 ID - LDAP 등 다양한 인증 방식, 암호화 기능을 제공한다.
- <b>스프링 배치</b> : 로깅/추적, 작업 통계, 실패 처리 등 배치 처리에 필요한 기본 기능을 제공한다. <br><br>
이 외에도 스프링 인티그레이션, 스프링 하둡, 스프링 소셜 등 다양한 프로젝트가 존재한다. 각 프로젝트에 대한 내용은 [https://spring.io](https://spring.io) 사이트를 참고하기 바란다.
* * *

# Maven
- Maven이란 자바용 프로젝트 관리 도구이이다.
- Maven은 Apache사에서 만든 빌드툴(build tool)이다
- pom.xml파일을 통해 정형화된 빌드 시스템으로 프로젝트 관리를 해준다.
- 프로젝트의 전체적인 라이프 사이클을 관리한다.

## 빌드
프로젝트를 위해 작성한 Java코드나 여러 자원들(.xml, .jar, .properties)를 JVM이나 톰캣 같은 WAS가 인식할 수 있는 구조로 패키징 하는 과정 및 결과물이다.<br>
또 단순히 컴파일 해주는 작업 뿐만 아니라, 테스팅, 검사, 배포까지 일련의 작업들을 통틀어 빌드라고 한다.<br>


# 개발환경 구축하기
- JDK 설치 및 JAVA_HOME 환경 변수 설정 : [1.JAVA/day01(환경구축,주석,자료형,변수)](https://github.com/to7485/Web1500/tree/main/1.JAVA/day01(%ED%99%98%EA%B2%BD%EA%B5%AC%EC%B6%95%2C%EC%A3%BC%EC%84%9D%2C%EC%9E%90%EB%A3%8C%ED%98%95%2C%EB%B3%80%EC%88%98))

## STS 설치하기
- STS란? : 스프링개발만을 위한 이클립스

![image](https://user-images.githubusercontent.com/54658614/236691353-94ee6992-8a08-43b2-85d6-30464baa560f.png)

- STS4를 눌러서 들어가지만 STS4를 사용하지는 않을것.

![image](https://user-images.githubusercontent.com/54658614/236691360-b94fb2b2-1fe1-48fb-8d9a-65c825b10fff.png)

- 아래로 스크롤을 해 STS3를 다운받을 수 있는 GITHUB로 이동한다.

![image](https://user-images.githubusercontent.com/54658614/236691367-198b6e30-d28a-4456-b765-2535efd224e3.png)

- 운영체제에 맞는 STS를 다운받는다.

![image](https://user-images.githubusercontent.com/54658614/236691376-8c797953-9b3d-46fe-b828-ea93785fa74b.png)

- 압축을 풀면 우리가 평소에 사용하던 이클립스처럼 실행할 수 있다.

![image](https://user-images.githubusercontent.com/54658614/236691451-6a7ce32a-4e3c-4dab-bd65-54ad9c43a5fa.png)

- work폴더를 설정하여 실행을 한다.

![image](https://user-images.githubusercontent.com/54658614/236691478-baac579e-4f8b-4133-a9c9-8c8b26a94383.png)

- 톰캣 9버전을 다운로드 받자

![image](https://user-images.githubusercontent.com/54658614/236691683-38cd0187-e370-4f61-b077-edcd822c399e.png)

- 왼쪽아래 VM어쩌구 되어있는거 클릭하고 delete를 눌러 삭제하자
- 그리고 다운받은 톰캣을 등록해주자.

![image](https://user-images.githubusercontent.com/54658614/236691710-420c67bf-46a2-4953-ba47-9afe935e0428.png)

- 톰캣 더블클릭하면 포트를 바꿀수 있다.

![image](https://user-images.githubusercontent.com/54658614/236691739-95cddfe4-bc35-4441-8ead-a8ff81093125.png)

- 뭐가 모자란다는 경고가 뜰 수 있다.

![image](https://user-images.githubusercontent.com/54658614/236692005-1d5d4e82-0197-42fa-82d3-7055381eb4bf.png)


![image](https://user-images.githubusercontent.com/54658614/236691986-082badff-f0b7-4751-aad8-4a833033716e.png)


# 스프링 프로젝트 생성하기

## Ex_날짜_Spring
- File > new > Spring Legacy Project
![image](https://user-images.githubusercontent.com/54658614/236692202-15d4f991-85d7-40f1-8c27-a70b7a7f885c.png)

- Spring MVC Project로 템플릿을 설정한다.

![image](https://user-images.githubusercontent.com/54658614/236692288-038b1003-f122-46ae-906c-17f8dcd8e4d7.png)

- 스프링 작업을 위한 메인 패키지가 미리 준비가 되어있어야 한다.
- 패키지는 3단계 이상 되어야 활성화가 된다.
- 단계는 .으로 구분한다.
- 패키지는 단순히 저장을 하는 용도만이 아니라 프로젝트끼리 구별하는 식별자로도 활용이된다.
- 

![image](https://user-images.githubusercontent.com/54658614/236692385-71be89b4-d28e-44b5-85d6-dd8ccc7311ea.png)


- 프로젝트가 만들어지면 api나 라이브러리와 같은 정보들이 모두 pom.xml파일에 기록이 되어있다.

![image](https://user-images.githubusercontent.com/54658614/236692694-aa542227-82db-459a-83bc-20951b036df0.png)

## pom.xml 수정하기
```
<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.hds</groupId>
	<artifactId>app</artifactId>
	<name>ex01</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
	<properties>
		<java-version>11</java-version>
		<org.springframework-version>5.1.20.RELEASE</org.springframework-version>
		<org.aspectj-version>1.9.0</org.aspectj-version>
		<org.slf4j-version>1.7.25</org.slf4j-version>
	</properties>
	<dependencies>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
			<exclusions>
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				 </exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
		<dependency>
		    <groupId>org.projectlombok</groupId>
		    <artifactId>lombok</artifactId>
		    <version>1.18.22</version>
		    <scope>provided</scope>
		</dependency>
		<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>	
		<!-- JSON -->
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.9.6</version>
		</dependency>
		
		<dependency>
			<groupId>com.fasterxml.jackson.dataformat</groupId>
			<artifactId>jackson-dataformat-xml</artifactId>
			<version>2.9.6</version>
		</dependency>
		
		<dependency>
			<groupId>com.google.code.gson</groupId>
			<artifactId>gson</artifactId>
			<version>2.8.2</version>
		</dependency>
		<!-- Logging -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<!-- Log4j -->
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.17</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.bgee.log4jdbc-log4j2 -->
		<dependency>
			<groupId>org.bgee.log4jdbc-log4j2</groupId>
			<artifactId>log4jdbc-log4j2-jdbc4</artifactId>
			<version>1.16</version>
		</dependency>
		<!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>
				
		<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
	
		<!-- Test -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>5.1.20.RELEASE</version>
		</dependency>
		
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>5.1.20.RELEASE</version>
		</dependency>
		
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
		</dependency>
		<!-- https://mvnrepository.com/artifact/javax.xml/jaxb-api -->
		<dependency>
			<groupId>javax.xml</groupId>
			<artifactId>jaxb-api</artifactId>
			<version>2.1</version>
		</dependency>
		<!-- https://mvnrepository.com.artifact/com.zaxxer/HikariCP -->
		<dependency>
			<groupId>com.zaxxer</groupId>
			<artifactId>HikariCP</artifactId>
			<version>2.7.4</version>
		</dependency>
		
		<!-- mybatis -->
		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.4.6</version>
		</dependency>
		
		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.3.2</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		
		<!-- Thumbnailator -->
		<dependency>
			<groupId>net.coobird</groupId>
			<artifactId>thumbnailator</artifactId>
			<version>0.4.8</version>
		</dependency>
		
		<!-- Quartz -->
		<dependency>
			<groupId>org.quartz-scheduler</groupId>
			<artifactId>quartz</artifactId>
			<version>2.3.0</version>
		</dependency>
		<dependency>
			<groupId>org.quartz-scheduler</groupId>
			<artifactId>quartz-jobs</artifactId>
			<version>2.3.0</version>
		</dependency>
	</dependencies>
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-eclipse-plugin</artifactId>
                <version>2.9</version>
                <configuration>
                    <additionalProjectnatures>
                        <projectnature>org.springframework.ide.eclipse.core.springnature</projectnature>
                    </additionalProjectnatures>
                    <additionalBuildcommands>
                        <buildcommand>org.springframework.ide.eclipse.core.springbuilder</buildcommand>
                    </additionalBuildcommands>
                    <downloadSources>true</downloadSources>
                    <downloadJavadocs>true</downloadJavadocs>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
                <configuration>
                    <source>11</source>
                    <target>11</target>
                    <compilerArgument>-Xlint:all</compilerArgument>
                    <showWarnings>true</showWarnings>
                    <showDeprecation>true</showDeprecation>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.2.1</version>
                <configuration>
                    <mainClass>org.test.int1.Main</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```
![image](https://user-images.githubusercontent.com/54658614/236693092-a17ca0d6-0d77-4562-a31f-a0386ca0a898.png)

![image](https://user-images.githubusercontent.com/54658614/236693108-a743437a-051b-4ae8-901c-beb28dc0741c.png)






