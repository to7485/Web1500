# 프레임워크란?
- Mybatis와 같이 뭔가를 만들기 위한 하나의 기본적인 틀을 의미한다.<br>이곳에 필요한 내용을 조립해서 쓰는 구조라고 생각하면 된다.

## 스프링 프레임워크란?
- 자바 플랫폼을 위한 오픈소스 어플리케이션 프레임워크로서 엔터프라이즈급 어플리케이션을 개발하기 위한 모든 기능을 종합적으로 제공하는 경량화된 솔루션이다.
- 엔터프라이즈급 개발이란 뜻대로만 풀이하면 기업을 대상으로 하는 개발이라는 뜻이다.
- 대규모 데이터 처리와 트랜잭션이 동시에 여러 사용자로 부터 행해지는 매우 큰 규모의 환경을 엔터프라이즈 환경을 뜻한다.

## Spring의 역사
- 스프링 시스템 이전에 EJB(엔터프라이즈 자바빈즈 Enterprise Java Beans)라는 프레임워크를 사용하고 있었는데, 너무 복잡하고 개발비용이 수억에서 수십억에 달한다.
- 그러나 Spring을 통해 EJB의 90% 이상의 기능을 구현할 수 있으며, 오픈소스 기반이기 때문에 제작비용이 들지 않는다는 장점이 있다.

### 전자정부표준프레임워크
- SpringFramework라고 생각해도 무방할 정도로 국내에 입찰되는 정부사업은 대부분 Spring을 사용한다고 보면 된다.

Spring은 아래 그림과 같이 기본 틀을 제공하고 필요한 기능들을 추가해서 사용하는 구조라고 생각하면 된다.<br>
( 로그인, 게시판 등 만들어져 있는 기능을 끼워넣어서 사용하면 되는데, 물론 원하는 모양으로 디자인 하려면 커스터마이징이 필요하긴 하다.)<br>

![image](https://user-images.githubusercontent.com/54658614/236684904-d4e2e78e-245c-421d-912f-ec0ffe9f4e48.png)

실제로 스프링 프레임워크를 이용해서 웹 어플리케이션을 개발할 때에는 스프링 프레임워크만 단독으로 사용하기 보다는 여러 스프링 관련 프로젝트를 함께 사용한다.<br>
현재 스프링을 주도적으로 개발하고 있는 피보탈(Pivotal)은 스프링 프레임워크뿐만 아니라 어플리케이션 개발에 필요한 다양한 프로젝트를 진행하고 있다. 이들 프로젝트 중 자주 사용되는 것은 다음과 같다.
- <b>스프링 데이터</b> : 적은 양의 코드로 데이터 연동을 처리할 수 있도록 도와주는 프레임워크이다. JPA, 몽고DB, 레디스 등 다양한 저장소 기술을 지원한다.
- <b>스프링 시큐리티</b> : 인증/인가와 관련된 프레임워크로서 웹 접근 제어, 객체 접근 제어, DB - 오픈 ID - LDAP 등 다양한 인증 방식, 암호화 기능을 제공한다.
- <b>스프링 배치</b> : 로깅/추적, 작업 통계, 실패 처리 등 배치 처리에 필요한 기본 기능을 제공한다. <br><br>

이 외에도 스프링 인티그레이션, 스프링 하둡, 스프링 소셜 등 다양한 프로젝트가 존재한다. 각 프로젝트에 대한 내용은 [https://spring.io](https://spring.io) 사이트를 참고하기 바란다.

* * *

## 스프링 프레임워크의 특징

### 1. 의존성 주입(DI : Dependency Injection)
- 어떤 객체가 사용하는 의존 객체를 직접 만들어 사용하는게 아니라, 주입받아 사용하는 방법이다.
- 예를 들어 장난감들은 배터리가 있어야 움직일 수 있다. 즉, 배터리에 의존을 하고 있다.
- 장난감들에게 배터리를 넣어주는 것을 의존성 주입이라고 한다.
- 각각의 계층이나 서비스들 간에 의존성이 존재할 경우 프레임워크가 서로 연결시켜준다.

### 3. 제어 역행(IoC : Inversion of Control)
- 제어의 주체가 개발자가 아닌 프레임워크라는 뜻으로 때에 따라 프레임워크가 작성된 코드를 호출하는 기술
- 객체의 생명주기의 관리까지 모든 객체에 대한 제어권을 프레임워크가 가진다.
- 일반적인 상황에서는 개발자가 new 연산자를 통해 객체를 생성하고 직접 제어를 해야했다.
- Spring에서는 xml파일 또는 어노테이션 방식으로 스프링 컨테이너에 Bean을 등록하기만 하면된다.
- Spring은 Bean객체의 생명주기(생성 -> 의존성 설정 -> 초기화 -> 소멸)를 전부 관리해준다.
- ##### Bean(빈)이란 무엇인가?
	- 스프링 IoC컨테이너가 관리하는 객체
	- 스프링 IoC컨테이너에 등록된 Bean들은 의존성 관리가 수월해진다.
	- 스프링 IoC컨테이너에 등록된 Bean들은 싱글톤의 형태이다.

### 4. 관점지향 프로그래밍(AOP : Aspect-Oriented Programming)
- 객체지향적으로 프로그래밍을 했음에도 로그, 트랜잭션, 성능확인 등 공통적인 관심사가 중복되는 문제점을 해결하기 위해 프록시 패턴을 사용하여 코드를 분리하여 관리하는 기술

### 5. POJO : Plane Old Java Object(순수 자바 객체)
- EJB등에서 사용되는 Java Bean이 아닌 필드와 Getter,Setter로 구성된 가장 순수한 형태의 기본 클래스이다.

	
* * *

# Maven
- Maven이란 자바용 프로젝트 관리 도구이이다.
- Maven은 Apache사에서 만든 빌드툴(build tool)이다
- pom.xml파일을 통해 정형화된 빌드 시스템으로 프로젝트 관리를 해준다.
- 필요한 라이브러리를 pom.xml에 정의해 놓으면 내가 사용할 라이브러리뿐만 아니라
- 해당 라이브러리가 작동하는 데에 필요한 다른 라이브러리들까지 관리하여 네트워크를 통해 자동 다운을 해준다.
- 프로젝트의 전체적인 라이프 사이클을 관리한다.

## 빌드
- 프로젝트를 위해 작성한 Java코드나 여러 자원들(.xml, .jar, .properties)를 JVM이나 톰캣 같은 WAS가 인식할 수 있는 구조로 패키징 하는 과정 및 결과물이다.
- 또 단순히 컴파일 해주는 작업 뿐만 아니라, 테스팅, 검사, 배포까지 일련의 작업들을 통틀어 빌드라고 한다.
- 빌드 순서는 compile -> test -> package순으로 진행된다.
- compile은 src/main/java 디렉토리 아래의 모든 소스 코드를 컴파일 하는 과정이다.
- test는 src/test/java, src/test/resources 테스트 자원 복사 및 테스트 소스를 컴파일 하는 과정이다.
- packaging단계는 컴파일과 테스트가 완료된 후에 jar, war같은 형태로 압축하는 작업이다.

## Maven의 특징
- 빌드 과정을 쉽게 만들기
- 정형화된 빌드 시스템 제공
- Maven은 Pom과 플러그인 세트를 사용하여 프로젝트를 빌드한다.
- 양질의 프로젝트 정보 제공
- 더 나은 개발

## 장점
- 편리한 의존성 라이브러리
- 정해진 빌드 방법을 사용하여 협업에서 유리하게 작용
- 다양한 플러그인을 통해 많은 작업이 자동화됨


![image](image/Maven.png)

## Maven LifeCycle

![image](image/MavenLifeCycle.png)


* * *

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

***

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

- com.korea.first

![image](https://user-images.githubusercontent.com/54658614/236692385-71be89b4-d28e-44b5-85d6-dd8ccc7311ea.png)

# 프로젝트의 기본 경로
|경로|설명|
|---|---|
|src/main/java|서버단 JAVA파일|
|src/test/java|단위테스트를 위한 JAVA파일|
|src/main/resources|관련 설정파일|
|src/test/resources|src/main/test 관련 설정 파일|
|src/main/webapp/WEB-INF/views|jsp, html파일경로|
|pom.xml|라이브러리 의존성 관리|
|src/main/webapp/WEB-INF/spring/appServlet/servlet-context.xml| 웹과 관련된 스프링 설정파일|
|src/main/webapp/WEB-INF/spring/root-context.xml| 스프링 객체 관련 설정 파일|

- 프로젝트를 처음 만들게 되면 예제들이 기본적으로 만들어져있다.
- 실행할 때는 프로젝트에 우클릭을 하여 run as > run on server로 실행하자

![image](https://user-images.githubusercontent.com/54658614/236693929-f5fee8d0-2986-4a58-8b22-af49df523c13.png)

- 프로젝트가 만들어지면 api나 라이브러리와 같은 정보들이 모두 pom.xml파일에 기록이 되어있다.

![image](https://user-images.githubusercontent.com/54658614/236692694-aa542227-82db-459a-83bc-20951b036df0.png)

# pom.xml
- Project Object Model의 약자로 프로젝트의 다양한 정보를 처리하기 위한 객체 모델이다.
- pom.xml에는 프로젝트 관리 및 빌드에 필요한 환경 설정, 의존성 관리 등의 정보를 기술한다.

## pom.xml의 최소한의 구성
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.example</groupId>
    <artifactId>maven-pom-project</artifactId>
    <version>1.0-SNAPSHOT</version>
</project>
```
- project,modelVersion, groupId, artifactId,version은 메이븐이 허용한 최소값이다.

## pom.xml의 기본속성 태그
```xml
<project></project>
```
- 프로젝트의 정보를 기술한다.

```xml
<modelVersion>4.0.0</modelVersion>
```
- 4.0.0이라고 적혀있는 것은 maven의 pom.xml의 모델 버전이다.
- Maven 1.x버전들은 3.0.0모델을 사용하였지만 Maven 2.x, Maven 3.x는 4.0.0버전을 사용한다.

```xml
<groupId>org.example</groupId>
```
- 프로젝트를 생성한 그룹명으로 제작사와 회사, 단체 등을 식별하기 위한것이다.

```xml
<artifactId>maven-pom-project</artifactId>
```
- 버전정보를 생략한 jar파일의 이름이다.

```xml
<version>4.0.0</version>
```
- 명시된 그룹의 artifact버전을 표기한다.
- 숫자와 점으로 이루어진(4.0.0)일반적인 버전 형태를 사용한다.

```xml
<packaging>jar</packaging>
```
- 프로젝트를 어떤 형태로 패키징할지 지정한다.(jar, war, zip등...)

```xml
<properties>
	<spring.maven.artifact.version>4.3.16.RELEASE</spring.maven.artifact.version>
</properties>
```
- pom.xml에서 사용하는 속성값들을 정의하고 pom내 어디서든 사용할 수 있다.

```xml
<dependencies>
	<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>${spring.maven.artifact.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.maven.artifact.version}</version>
	</dependency>
</dependencies>
```
- \<dependencies\>는 pom.xml 태그중 최상위 속성을 가진 태그중 하나이며, 프로젝트와 의존관계에 있는 라이브러리를 모아서 관리하는 곳이다.
- 각각의 의존 라이브러리들의 정보는 \<dependency\>태그를 사용하여 작성한다.
- 참고로 \<dependencies\>태그는 라이브러리들을 모아서 관리하는 곳이기에 \<dependency\>태그를 사용하여 필요한 라이브러리를 기술하면 된다.

```xml
<dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
</dependency>
```
- \<dependency\>태그는 라이브러리의 정보를 기술한다.
- groupId,artifactId,version은 위에 pom.xml의 기본태그에서 설명한 바와 같다.
- <scope>태그는 이 라이브러리가 이용되는 범위를 지정하는 것이다.

## Build 수행을 위한 pom.xml태그와 설정
- 빌드 구성은 pom.xml에서 \<plugin\>설정에 의해 실행된다.
- \<plugin\> : 빌드에서 사용할 플러그인

### maven에서 플러그인이란?
- Maven은 여러 플러그인으로 구성되어 있으며, 각각의 플러그인은 하나 이상의 명령을 포함하고 있다.


## pom.xml 수정하기
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.korea</groupId>
	<artifactId>kkk</artifactId>
	<name>PomTest</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
	<properties>


//////////////////////////////////////////////////////////////////////////////////////////////////////
		<java-version>11</java-version>
		<org.springframework-version>5.1.20.RELEASE</org.springframework-version>
		<org.aspectj-version>1.9.0</org.aspectj-version>
		<org.slf4j-version>1.7.25</org.slf4j-version>
//////////////////////////////////////////////////////////////////////////////////////////////////////


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
				
		<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
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
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.15</version>
			<exclusions>
				<exclusion>
					<groupId>javax.mail</groupId>
					<artifactId>mail</artifactId>
				</exclusion>
				<exclusion>
					<groupId>javax.jms</groupId>
					<artifactId>jms</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jdmk</groupId>
					<artifactId>jmxtools</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jmx</groupId>
					<artifactId>jmxri</artifactId>
				</exclusion>
			</exclusions>
			<scope>runtime</scope>
		</dependency>

		<!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>



//////////////////////////////////////////////////////////////////////////////////////////////////////				
		<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>4.0.1</version>
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
//////////////////////////////////////////////////////////////////////////////////////////////////////



		<!-- Test -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
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

///////////////////////////////////////////////////////////////////////////////////
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
///////////////////////////////////////////////////////////////////////////////////


            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.2.1</version>
                <configuration>
                    <mainClass>org.test.int1.Main</mainClass>
                </configuration>
            </plugin>


///////////////////////////////////////////////////////////////////////////////////
            <plugin>
            	<groupId>org.apache.maven.plugins</groupId>
            	<artifactId>maven-war-plugin</artifactId>
            	<version>3.2.0</version>
            	<configuration>
            		<failOnMissingWebXml>false</failOnMissingWebXml>
            	</configuration>
            </plugin>
////////////////////////////////////////////////////////////////////////////////////


        </plugins>
    </build>
</project>
```

- 안된다면 톰캣의 jstl 관련 라이브러리 옮겨주기

## home.jsp 수정하기
- 아마 한글이 깨질것이다 예제파일에는 인코딩 타입이 빠져있다 추가해주자.
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ page session="false" %>
<html>
<head>
	<title>Home</title>
</head>
<body>
<h1>
	Hello world!  
</h1>

<P>  The time on the server is ${serverTime}. </P>
</body>
</html>

```
- 실행하면 home.jsp가 실행되어 다음과 같은 화면이 뜬다.

![image](https://user-images.githubusercontent.com/54658614/236734497-eac24649-c08d-4924-a053-7aa2a163cdaf.png)

# home.jsp가 어떻게 실행이 되는가
- ServerTime이라고 하는 부분이 EL표기법으로 묶여 있다 그 말은 어딘가의 영역에 묶여있다는 뜻.
- src/main/java에 HomeController.java로 가보자

![image](https://user-images.githubusercontent.com/54658614/236735580-1e6a03ec-1464-4f02-8f34-3cd6b62a7ea9.png)

```java
package com.korea.test;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * Handles requests for the application home page.
 */

@Controller
public class HomeController {
	
	private static final Logger logger = LoggerFactory.getLogger(HomeController.class);
	
	/**
	 * Simply selects the home view to render by returning its name.
	 */
	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home(Locale locale, Model model) {
		logger.info("Welcome home! The client locale is {}.", locale);
		
		Date date = new Date();
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);
		
		String formattedDate = dateFormat.format(date);
		
		//model이 request와 비슷한 역할을 한다.
		model.addAttribute("serverTime", formattedDate );
		model.addAttribute("hello","스프링은 처음입니다.");
		
		
		return "home";
	}
	
}

```

# Spring Framework의 구동순서

![SpringFramework구동 순서](image/spring.png)

1. 웹 어플리케이션이 실행되면 Tomcat(WAS)에 의해 web.xml이 실행된다.

2. web.xml에 등록되어있는 ContextLoaderListener생성
- ContextLoaderListener는 ServletContextListener 인터페이스를 구현하고있으며, ApplicationContext를 생성한다. 
	- ApplicationContext는 별도의 설정 정보를 참고하고 IoC를 적용하여 빈의 생성, 관계설정 등의 제어 작업을 총괄한다.

3. ContextLoaderListener가 root-context를 로딩
	- * ContextLoaderListener : 서블릿을 초기화하는 용도로 사용

4. root-contex에 등록되어있는 설정에 따라 Spring Container가 구동

5. 클라이언트로부터 Web Appllication에 요청을 받는다.

6. DispatcherServlet생성
	- DispatcherServlet : 서블릿 컨테이너의 가장 앞단에서 HTTP 프로토콜로 들어오는 모든 요청을 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러이다.
	- 프론트 컨트롤러 : 서블릿 컨테이너의 제일 앞에서 서버로 들어오는 클라이언트의 모든 요청을 받아서 처리해주는 컨트롤러
		- HandlerMapping이 컨트롤러를 찾아주는 기능을 한다. 스프링이 제공하는 Handler Mapping방법은 5가지이다.
			- BeanNameUrlHandlerMapping : 빈의 이름에 들어있는 URL을 HTTP 요청의 URL과 비교해서 일치하는 빈을 찾아준다. 가장 직관적이고 사용하기 쉬운 핸들러 매핑 전략이다.
			- ControllerBeanNameHandlerMapping : BeanNameUrlHandlerMapping과 유사하지만 빈 이름을 URL 형태로 짓지 않아도 된다는 것이 차이점이다. 빈 이름 앞에 자동으로 / 이 붙여져 URL에 매핑된다.
			- ControllerClassNameHandlerMapping : 빈의 클래스 이름을 URL에 매핑해주는 매핑 클래스.<br> 기본적으로 클래스 이름을 모두 사용하지만 클래스 이름이 Controller로 끝날 경우 Controller를 뺀 나머지 이름을 URL에 매핑해준다.
			- SimpleUrlHandlerMapping : URL과 컨트롤러 매핑정보를 한곳에 모아놓을 수 있는 전략이다.<br> 매핑정보는 SImpleUrlHandlerMapping 빈의 프로퍼티에 넣어준다.<br>디폴트 핸들러 매핑 전략이 아니기도 하고 프로퍼티에 매핑정보를 직접 넣어줘야 하므로 SimpleUrlHandlerMapping 빈을 등록해야 사용할 수 있다.
			- DefaultAnnotationHandlerMapping : @RequestMapping 어노테이션을 이용해 매핑하는 전략이다. 클래스는 물론 메서드 단위로도 URL을 매핑할 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/c110197d-ad20-47ee-81f1-2e1d82e70589)

7. DispatcherServlet이 servlet-context를 로딩

8. 두번째 Spring Container가 구동되며, 응답에 맞는 Controller들이 동작한다.

## home.jsp에 코드 추가하고 실행하기
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ page session="false" %>
<html>
<head>
	<title>Home</title>
</head>
<body>
<h1>
	Hello world! / ${hello} 
</h1>

<P>  The time on the server is ${serverTime}. </P>
</body>
</html>

```
# Lombok 라이브러리
- Lombok(롬복)을 이용하면 JAVA개발 시 setter/getter, 생성자 등을 @Data등의 어노테이션을 통해 자동으로 생성해준다.

![image](https://user-images.githubusercontent.com/54658614/236693997-449b3281-e657-4f54-b5be-b0259d19a565.png)

![image](https://user-images.githubusercontent.com/54658614/236694089-00091f61-7b01-49b0-9e09-e58b5270d275.png)

- 더블클릭하여 실행이 안된다면 cmd를 켜고 롬복파일이 있는 경로까지 이동하여 java -jar lombok.jar 명령어 입력하여 실행하기

![image](https://user-images.githubusercontent.com/54658614/236694094-e976f935-36c0-4803-8d44-6389b5d11ffd.png)

- ide의 경로 잡아주기

![image](https://user-images.githubusercontent.com/54658614/236694237-09a7f41d-2b58-4068-a9ea-ecc3dc642f13.png)

- 혹시 모르니 pom.xml에도 알려주자

```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.26</version>
    <scope>provided</scope>
</dependency>
```

|어노테이션|설명|
|---|---|
|@Getter|getter메서드 생성|
|@Setter|setter메서드 생성|
|@NoArgsConstructor|파라미터 없는 기본생성자|
|@AllArgsConstructor|어노테이션은 모든 필드 값을 파라미터로 받는 생성자를 만들어줍니다.|
|@RequiredArgsConstructor|final이나 @NonNull인 필드 값만 파라미터로 받는 생성자를 만들어줍니다.|
|@Data|@Getter, @Setter, @RequiredArgsConstructor, @ToString, @EqualsAndHashCode을 한꺼번에 설정|


# MVC 디자인 패턴 이론
- MVC는 Model, View, Controller의 약자로, 클라이언트와 상호작용하는 소프트웨어를 설계함에 있어 세가지 요소로 나누는 것을 말한다.

#### Model
- Model은 어플리케이션의 정보, 데이터의 가공을 책임지여 데이터베이스와 상호작용하여 비즈니스 로직을 처리하는 모듈
- 사용자가 이용하려는 모든 데이터를 가지고 있어야 한다.
- View(뷰) 또는 Controller(컨트롤러)에 대한 어떤 정보도 알 수 없어야 한다.
- 변경이 일어났을 때 처리 방법을 구현해야 한다.
- 모델은 재사용이 가능해야 하며 다른 인터페이스에서도 변하지 않아야 한다.

#### View
- View는 클라이언트 단에서 보여지는 결과화면을 반환하는 모듈
- Model(모델)이 가지고 있는 데이터를 저장하면 안된다.
- 데이터를 받안 단순히 화면에 표시해주는 역할만 가진다.
- 재사용이 가능하게끔 설계를 해야 하며 다른 정보들을 표현할 때 쉽게 설계해야 한다.

#### Controller
- Controller는 client로부터 request가 들어왔을 때 그 입력을 처리하고 어떤 로직을 실행시킬지 Model(모델)과 View(뷰)를 연결해주며 제어하는 모듈
- Model(모델)또는 View(뷰)에 대한 정보를 알아야 한다.
- Model(모델)또는 View(뷰)의 변경을 인지하여 대처를 해야 한다.
- 모델이나 뷰의 변경을 통지 받으면 이를 해석해서 각각의 구성 요소에게 통지해야 한다.
- 어플리케이션의 메인 로직을 담당한다.

## 코드의 분리
1. 관심사
2. 변하는것과 (잘)변하지 않는것은 분리할것
3. 중복코드 -> 별도의 메서드로 만들어서 분리하기

## 객체 지향 설계 5대 원칙 - SOLID
### 1. 단일 책임원칙(SRP, Single Responsibility Principle)
- 하나의 메서드는 하나의 관심사(책임)만 가져야 한다.
### 2. 개방 폐쇄 원칙(OCP, Open-Closed Principle)
- 상속에는 Open, 변경에는 Close 해야한다.
- 코드를 변경할일 있으면 변경하지 말고 웬만하면 상속을 통해서 변경해라
### 3. 리스코프 치환 원칙(LSP, Liskov Subtitution Principle)
- 같은 조상의 다른 클래스로 바꿔도 동작해야 한다(다형성)
### 4. 인터페이스 분리 원칙(ISP, Interface Segregation Principle)
- 유사한 인터페이스가 있더라도 목적이 다르면 분리해야 한다.

### 5. 의존관계 역전 원칙(DIP, Dependency Inversion Principle)
- 추상화에 의존한 코드를 작성해야 한다.
- 코드가 너무 구체적이면 변경에 불리하다.


## com.korea.first 패키지에 YoilTellerMVC클래스 생성하기

```java
@Controller
public class YoilTeller {
    @RequestMapping("/getYoil") // http://localhost:8080/ch2/getYoil?year=2021&month=10&day=1
	
    //    public static void main(String[] args) {
    public void main(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 1. 입력
        String year = request.getParameter("year");
        String month = request.getParameter("month");
        String day = request.getParameter("day");

        int yyyy = Integer.parseInt(year);
        int mm = Integer.parseInt(month);
        int dd = Integer.parseInt(day);

        // 2. 처리 -> 잘 안변함
        Calendar cal = Calendar.getInstance();
        cal.set(yyyy, mm - 1, dd);

        int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK);
        char yoil = " 일월화수목금토".charAt(dayOfWeek);   // 일요일:1, 월요일:2, ... 

        // 3. 출력 -> 자주 변경될 수 있음
        response.setContentType("text/html");    // 응답의 형식을 html로 지정
        response.setCharacterEncoding("utf-8");  // 응답의 인코딩을 utf-8로 지정
        PrintWriter out = response.getWriter();  // 브라우저로의 출력 스트림(out)을 얻는다.
        out.println("<html>");
        out.println("<head>");
        out.println("</head>");
        out.println("<body>");
        out.println(year + "년 " + month + "월 " + day + "일은 ");
        out.println(yoil + "요일입니다.");
        out.println("</body>");
        out.println("</html>");
        out.close();
    }
}
```

## 입력의 분리
- java에서 기본으로 제공하는 reflection api가 메서드가 가지고 있는 정보를 읽는다.
- 넘어온 정보를 매개변수에 알아서 집어넣어준다.

```java
    @RequestMapping("/getYoil") // http://localhost:8080/ch2/getYoil?year=2021&month=10&day=1
    //    public static void main(String[] args) {
    public void main(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 1. 입력
        String year = request.getParameter("year");
        String month = request.getParameter("month");
        String day = request.getParameter("day");

        int yyyy = Integer.parseInt(year);
        int mm = Integer.parseInt(month);
        int dd = Integer.parseInt(day);

```
↓↓↓↓↓↓↓↓↓↓↓

```java
@RequestMapping("/getYoil") // http://localhost:8080/ch2/getYoil?year=2021&month=10&day=1
    //    public static void main(String[] args) {
    public void main(int year, int month, int day, HttpServletResponse response) throws IOException {

        /*
		스프링이 타입변환까지 해주기때문에 필요가 없다.
        int yyyy = Integer.parseInt(year);
        int mm = Integer.parseInt(month);
        int dd = Integer.parseInt(day);
		*/
```

## 출력의 분리
- 변하는 것과 변하지 않는 것의 분리

```java
@Controller
public class YoilTeller {
    @RequestMapping("/getYoil") // http://localhost:8080/ch2/getYoil?year=2021&month=10&day=1
    //    public static void main(String[] args) {
    //public void main(int year, int month, int day, Model model) throws IOException {
	public String main(int year, int month, int day, Model model) throws IOException {	

        // 2. 처리 -> 잘 안변함
        Calendar cal = Calendar.getInstance();
        cal.set(yyyy, mm - 1, dd);

        int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK);
        char yoil = " 일월화수목금토".charAt(dayOfWeek);   // 일요일:1, 월요일:2, ... 

		//내가정보를 보여주고 싶은 페이지를 지정한다.
		return "yoil"; // /WEB-INF/views/yoil.jsp


        // 3. 출력 -> 자주 변경될 수 있음
		//여기에 작성하지 않고 JSP에 작성을 한다.
        // response.setContentType("text/html");    // 응답의 형식을 html로 지정
        // response.setCharacterEncoding("utf-8");  // 응답의 인코딩을 utf-8로 지정
        // PrintWriter out = response.getWriter();  // 브라우저로의 출력 스트림(out)을 얻는다.
        // out.println("<html>");
        // out.println("<head>");
        // out.println("</head>");
        // out.println("<body>");
        // out.println(year + "년 " + month + "월 " + day + "일은 ");
        // out.println(yoil + "요일입니다.");
        // out.println("</body>");
        // out.println("</html>");
        // out.close();
    }
}
```

## 처리부분 메서드로 만들기
```java
package com.korea.first;

import java.io.IOException;
import java.util.Calendar;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;


@Controller
public class YoilTellerMVC {
    @RequestMapping("/getYoil") // http://localhost:8080/ch2/getYoil?year=2021&month=10&day=1
    //    public static void main(String[] args) {
    //public void main(int year, int month, int day, Model model) throws IOException {
	public String main(int year, int month, int day, Model model) throws IOException {	

        // 2. 처리 -> 잘 안변함
    	
    	//유효성 검사
    	if(!isValid(year,month,day)) {
    		return "yoilError"; //넘어온 값이 올바르지 않다면 넘어갈 페이지
    	}

    	char yoil = getYoil(year,month,day);


		//내가정보를 보여주고 싶은 페이지를 지정한다.
		return "yoil"; // /WEB-INF/views/yoil.jsp

    }
    
    //현재클래스에서만 사용하기 위해 private으로 설정
    private boolean isValid(int year, int month, int day) {
		// TODO Auto-generated method stub
		return true;
	}

	private char getYoil(int year, int month, int day) {
        Calendar cal = Calendar.getInstance();
        cal.set(year, month - 1, day);
        
        int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK);
        char yoil = " 일월화수목금토".charAt(dayOfWeek);   // 일요일:1, 월요일:2, ... 
        
        return yoil;
    }  
}

```

### src/main/webapp/WEB-INF/views에 yoil.jsp,yoilError.jsp 만들기

```jsp
<%@ page contentType="text/html;charset=utf-8" %>
<html>
<head>
	<title>YoilTellerMVC</title>
</head>
<body>
<h1>${year}년 ${month}월 ${day}일은 ${yoil}요일입니다.</h1>
</body>
</html>
```

```jsp
<%@ page contentType="text/html;charset=utf-8" %>
<html>
<head>
	<title>YoilTellerMVC</title>
</head>
<body>
<h1>잘못된 요청입니다. 다시 요청해 주세요.</h1>
<h1>year, month, day를 모두 올바른 값으로 입력하셔야합니다. </h1>
</body>
</html>
```
- 컨트롤러에서 작업을 한 부분이 JSP까지 넘어오지 않았다.


### 데이터 model 객체에 묶기
- 더이상 response객체도 필요가 없고 Model 객체가 필요하다.
```java
 @RequestMapping("/getYoil") // http://localhost:8080/ch2/getYoil?year=2021&month=10&day=1 주소창에 입력하기
    //    public static void main(String[] args) {
    //public void main(int year, int month, int day, Model model) throws IOException {
	public String main(int year, int month, int day, Model model) throws IOException {	

        // 2. 처리 -> 잘 안변함
    	
    	//유효성 검사
    	if(!isValid(year,month,day)) {
    		return "yoilError"; //넘어온 값이 올바르지 않다면 넘어갈 페이지
    	}

    	char yoil = getYoil(year,month,day);

		//3. 계산한 결과를 model에 저장
	    model.addAttribute("year",  year);     	
      	model.addAttribute("month", month);     	
      	model.addAttribute("day",   day);
      	model.addAttribute("yoil", yoil);   

		//내가정보를 보여주고 싶은 페이지를 지정한다.
		return "yoil"; // /WEB-INF/views/yoil.jsp

    }

	...

```

- 다시 확인하기


![image](image/mvc.png)

### 메서드의 반환값이 View로 바뀐 이유
- servlet-context.xml이 웹과 관련된 설정을 담고 있다.
- 설정은 자신의 마음대로 바꿀수 있다.

![image](image/servlet-context.png)

```xml
...

<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" /> <!-- 접두사로 붙여라 -->
		<beans:property name="suffix" value=".jsp" /> <!-- 접미사로 붙여라 -->
	</beans:bean>

...

```

### 스프링 MVC로 나누기 총정리
1. 입력받을 값들을 매개변수로 선언해서 값들을 받는다.
2. 메서드 내부에서 받은 값들을 처리한다.
3. 처리한 결과를 model에 저장한다음 view를 반환해준다.
4. DispatcherServlet이 모델객체의 값을 읽어서 View에 전달해준다.

## 메서드의 반환형이 될 수 있는 타입
### 1. void
##### 매핑된 url에 의해 View의 이름이 결정된다.
- 잘은 안쓰지만 가능은 하다.
- View(jsp)의 이름이 바뀌었을때 매핑만 바꾸면 되서 관리해야 할 부분이 줄어든다.
```java
@Controller
public class YoilTellerMVC {
    @RequestMapping("/getYoil")
	public void main(int year, int month, int day, Model model) throws IOException {	

    	if(!isValid(year,month,day)) {
    		return "yoilError";
    	}

    	char yoil = getYoil(year,month,day);
    	
	    model.addAttribute("year",  year);     	
      	model.addAttribute("month", month);     	
      	model.addAttribute("day",   day);
      	model.addAttribute("yoil", yoil);    

		//return "yoil";

    }
```
- 매핑이 잘 되는지 getYoil.jsp 만들어보기


### 2. ModelandView
- 매개변수로 model을 받을 수 있기때문에 얘도 잘 안쓰지만 알아만두자.
```java
@Controller
public class YoilTellerMVC2 {
    @RequestMapping("/getYoil")
	public ModelAndView main(int year, int month, int day) throws IOException {	
    	
    	ModelAndView mv = new ModelAndView();

    	if(!isValid(year,month,day)) {
    		mv.setViewName("yoilError");
    		return mv;
    	}

    	char yoil = getYoil(year,month,day);
    	
    	mv.addObject("year",  year);     	
    	mv.addObject("month", month);     	
    	mv.addObject("day",   day);
    	mv.addObject("yoil", yoil);
    	
    	//결과를 보여줄 View를 지정해야 한다.
    	mv.setViewName("yoil");
    	
    	//메서드는 하나의 값만 반환할 수 있기 때문에 mv객체에 값과 view를 같이 넣어서 반환한다.
    	return mv;
    }
```

## MVC패턴의 원리

###  스프링이 매개변수를 얻어오는 두가지 방법
1. Reflection API
- 먼저 Reflection API를 통해서 매개변수 이름을 얻어오려고 시도한다.
2. Class File
- 실패하면 클래스파일을 직접 읽어서 매개변수의 이름을 가져온다.


#### JavaReflection API 방식
[Java Reflection API  설명 링크](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EB%88%84%EA%B5%AC%EB%82%98-%EC%89%BD%EA%B2%8C-%EB%B0%B0%EC%9A%B0%EB%8A%94-Reflection-API-%EC%82%AC%EC%9A%A9%EB%B2%95)

```java
package com.korea.first;

import java.lang.reflect.Method;
import java.lang.reflect.Parameter;
import java.util.StringJoiner;

public class MethodInfo {
	public static void main(String[] args) throws Exception{

		//1. YoilTellerMVC클래스의 객체를 생성

		//Java.lang.Class 클래스
		//클래스의 정보를 얻어오기위한 클래스이다.
		//forName() : 클래스의 파일명을 인자로 넣어주면 해당하는 클래스를 반환해준다.
		Class clazz = Class.forName("com.korea.first.YoilTellerMVC");
		
		//새로운 객체를 생성하는 방법
		//프로그램이 동작하는 시점에 이름이 결정되는 경우에 사용한다.
		Object obj = clazz.newInstance();
		

		//2. 모든 메서드 정보를 가져와서 배열에 저장

		//getDelcaredmethods()
		//상속한 메서드를 제외하고 접근지정자에 상관없이 모든 메서드를 가져온다.
		Method[] methodArr = clazz.getDeclaredMethods();
		
		for(Method m : methodArr) {
			String name = m.getName();//메서드의 이름
			Parameter[] paramArr = m.getParameters(); //매개변수
//			Class[] paramTypeArr = m.getParameterTypes();
			Class returnType = m.getReturnType();//반환 타입
			
			//", " : 구분자
			//"(" : 접두사
			//")" : 접미사
			StringJoiner paramList = new StringJoiner(", ", "(", ")");
			
			for(Parameter param : paramArr) {
				String paramName = param.getName();
				Class  paramType = param.getType();
				
				paramList.add(paramType.getName() + " " + paramName);
			}
			
			System.out.printf("%s %s%s%n", returnType.getName(), name, paramList);
		}
	} // main
}
```
- 매변수의 이름이 year,month,day인데 arg0, arg1 이런식으로 저장이 되어있다.
- 스프링은 매개변수의 타입은 중요하지만 이름은 중요하지 않게 생각한다.
- javac -parameters 옵션을 줘야 한다. 그래야 컴파일러가 매개변수의 이름까지 저장을한다.
- windows - preferences - compiler - Store information about method... 체크하기

#### class 파일로 매개변수 얻어오는지 확인하기
- show view -> navigator 클릭
- target에 빌드된 .class파일이 저장이 된다.
- 클래스파일에서 우클릭하고 openwith -> class File Viewer를 통해서 해석해서 보여준다.
- local variable table에 매개변수의 이름을 읽어서 스프링이 값을 넣어준다.

```java
/* [실행결과]
java.lang.String main(java.lang.String year, java.lang.String month, java.lang.String day, org.springframework.ui.Model model)

boolean isValid(int year, int month, int day)
*/
```




## @RequestParam, @ModelAttribute
### @RequestParam
- 요청의 파라미터를 연결할 매개변수에 붙히는 어노테이션

```java
@RequestParam("가져올 데이터의 이름") [데이터타입] [가져온데이터를 담을 변수]
```

```java
package com.korea.study;

import java.util.Date;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class RequestParamTest {
	@RequestMapping("/requestParam")
	public String main(HttpServletRequest request) {
		String year = request.getParameter("year");
//		http://localhost/ch2/requestParam         ---->> year=null
//		http://localhost/ch2/requestParam?year=   ---->> year=""
//		http://localhost/ch2/requestParam?year    ---->> year=""
		System.out.printf("[%s]year=[%s]%n", new Date(), year);
		return "yoil";
	}

	@RequestMapping("/requestParam2")
	//생략가능
	//name="year"는 파라미터 이름
	//required 필수여부 true: 반드시 값이 넘어와야함
//	public String main2(@RequestParam(name="year", required=false) String year) {   // 아래와 동일 
	public String main2(String year) {   
//		http://localhost/ch2/requestParam2         ---->> year=null
//		http://localhost/ch2/requestParam2?year    ---->> year=""
		System.out.printf("[%s]year=[%s]%n", new Date(), year);
		return "yoil";
	}

	@RequestMapping("/requestParam3")
//		public String main3(@RequestParam(name="year", required=true) String year) {   // 아래와 동일 
		public String main3(@RequestParam String year) {   
//		http://localhost/ch2/requestParam3         ---->> year=null   400 Bad Request. required=true라서 
//		http://localhost/ch2/requestParam3?year    ---->> year=""
		System.out.printf("[%s]year=[%s]%n", new Date(), year);
		return "yoil";	
	}

	@RequestMapping("/requestParam4")
	public String main4(@RequestParam(required=false) String year) {   
//		http://localhost/ch2/requestParam4         ---->> year=null 
//		http://localhost/ch2/requestParam4?year    ---->> year=""   
		System.out.printf("[%s]year=[%s]%n", new Date(), year);
		return "yoil";
	}

	@RequestMapping("/requestParam5")
	public String main5(@RequestParam(required=false, defaultValue="1") String year) {   
//		http://localhost/ch2/requestParam5         ---->> year=1   
//		http://localhost/ch2/requestParam5?year    ---->> year=1   
		System.out.printf("[%s]year=[%s]%n", new Date(), year);
		return "yoil";
	}
	
// =======================================================================
	
	@RequestMapping("/requestParam6") 
	public String main6(int year) {   
//		http://localhost/ch2/requestParam6        ---->> 500 java.lang.IllegalStateException: Optional int parameter 'year' is present but cannot be translated into a null value due to being declared as a primitive type. Consider declaring it as object wrapper for the corresponding primitive type.
//		http://localhost/ch2/requestParam6?year   ---->> 400 Bad Request, nested exception is java.lang.NumberFormatException: For input string: "" 
		System.out.printf("[%s]year=[%s]%n", new Date(), year);
		return "yoil";
	}
	
	@RequestMapping("/requestParam7") 
	public String main7(@RequestParam int year) {   
//		http://localhost/ch2/requestParam7        ---->> 400 Bad Request, Required int parameter 'year' is not present
//		http://localhost/ch2/requestParam7?year   ---->> 400 Bad Request, nested exception is java.lang.NumberFormatException: For input string: "" 
		System.out.printf("[%s]year=[%s]%n", new Date(), year);
		return "yoil";
	}

	@RequestMapping("/requestParam8") 
	public String main8(@RequestParam(required=false) int year) {   
	//	http://localhost/ch2/requestParam8        ---->> 500 java.lang.IllegalStateException: Optional int parameter 'year' is present but cannot be translated into a null value due to being declared as a primitive type. Consider declaring it as object wrapper for the corresponding primitive type.
	//	http://localhost/ch2/requestParam8?year   ---->> 400 Bad Request, nested exception is java.lang.NumberFormatException: For input string: "" 
		System.out.printf("[%s]year=[%s]%n", new Date(), year);
		return "yoil";
	}
	
	@RequestMapping("/requestParam9") 
	public String main9(@RequestParam(required=true) int year) {   
	//	http://localhost/ch2/requestParam9        ---->> 400 Bad Request, Required int parameter 'year' is not present
	//	http://localhost/ch2/requestParam9?year   ---->> 400 Bad Request, nested exception is java.lang.NumberFormatException: For input string: "" 
		System.out.printf("[%s]year=[%s]%n", new Date(), year);
		return "yoil";
	}
	
	@RequestMapping("/requestParam10")   
	public String main10(@RequestParam(required=true, defaultValue="1") int year) {   
	//	http://localhost/ch2/requestParam10        ---->> year=1   
	//	http://localhost/ch2/requestParam10?year   ---->> year=1   
		System.out.printf("[%s]year=[%s]%n", new Date(), year);
		return "yoil";
	}

	@RequestMapping("/requestParam11")   
	public String main11(@RequestParam(required=false, defaultValue="1") int year) {   
//		http://localhost/ch2/requestParam11        ---->> year=1   
//		http://localhost/ch2/requestParam11?year   ---->> year=1   
		System.out.printf("[%s]year=[%s]%n", new Date(), year);
		return "yoil";
	}
} // class
```

<hr>

# 어노테이션 기반의 설정파일 생성하기

## config 패키지 생성하기
- web.xml, root-context.xml, servlet-context.xml 삭제하기

![image](image/delete.png)

- src/main/java에 config패키지 생성하기

## web.xml 역할을 하는 WebInitializer.java 생성하기
```java
package config;

import javax.servlet.Filter;

import org.springframework.web.filter.CharacterEncodingFilter;
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class WebInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

	//AbstractAnnotationConfigDispatcherServletInitializer
	//스프링에서 제공하는 클래스로 웹 어플리케이션의 초기화를 위한 편리한 방법을 제공한다.
	//이 클래스를 사용하면 자바 기반 설정 클래스를 이용하여 DispatcherServlet 및 ContextLoaderListener를 등록할 수 있다.
	//이를 통해 web.xml을 사용하지 않고도 웹 어플리케이션의 초기화를 설정할 수 있다.
	//해당 클래스를 상속받아 필요한 설정을 오버라이드 하여 사용한다.

	//getRootConfigClasses()
	//프로젝트의 모델 영역 설정을 담당한다.
	//데이터베이스 연결풀(DBCP), Mybatis, Mybatis매퍼 등과 같은 로직 설정을 담당하는 메서드
	@Override
	protected Class<?>[] getRootConfigClasses() {
		return new Class[] { RootContext.class };
	}

	//.class : 클래스 리터럴
	//클래스 그 차체를 참조하는 구문
	//예를들어 String.class는 String클래스의  Class객체를 참조한다.
	//Class stringClass = String.class;
	//System.out.println(stringClass.getName()); java.lang.String

	//Class 클래스
	//자바의 리플렉션 API의 일부로 클래스와 인터페이스의 메타데이터에 접근할 수 있게 해준다.
	//Class객체는 특정 클래스에 대한 정보를 캡슐화하며, 해당클래스의 이름, 슈퍼클래스, 구현한 인터페이스 메서드, 생성자등의 정보를 제공한다.

	//getName(): 클래스의 완전한 이름 (패키지 이름 포함)을 반환합니다.
	//getSuperclass(): 슈퍼클래스의 Class 객체를 반환합니다.
	//getMethods(): 클래스의 모든 public 메서드를 반환합니다.
	//getDeclaredMethods(): 모든 메서드 (public, protected, default, private)를 반환합니다.
	//getConstructors(): 클래스의 public 생성자를 반환합니다.

	//getServletConfigClasses
	//DispatcherServlet이 사용할 설정 클래스를 반환한다.
	//Spring MVC 웹 영역 설정을 담당한다.
	//VIEW와 Controller 관련 설정을 담당하는 메서드

	@Override
	protected Class<?>[] getServletConfigClasses() {
		return new Class[] { ServletContext.class };
	}
	
	//getServletMappings
	//DispatcherServlet의 URL 패턴을 지정한다.
	//모든 요청을 처리하도록 "/"로 설정되어있다.
	@Override
	protected String[] getServletMappings() {
		return new String[] { "/" };
	}

	//filter
	//Client의 요청이 Servlet에 도달하기 전이나 후에 요청 및 응답 데이터를 변형하거나
	//추가작업을 수행하는데 사용되는 자바 컴포넌트를 의미한다.
	@Override
    	protected Filter[] getServletFilters() {
        CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter();
        characterEncodingFilter.setEncoding("UTF-8");
        characterEncodingFilter.setForceEncoding(true);

        return new Filter[] { characterEncodingFilter };
    }
}

```
## root-context.xml역할을 하는 RootContext.java 생성하기
```java
package config;

import org.springframework.context.annotation.Configuration;

@Configuration
public class RootContext {

}

```

## servlet-context.xml역할을 하는 ServletContext.java만들기
```java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;
import org.springframework.web.servlet.view.JstlView;

@Configuration
//@Configuration : 설정파일을 만들기 위한 어노테이션, Bean을 등록하기 위한 어노테이션

@EnableWebMvc
//@EnableWebMvc : Enable로 시작하는 어노테이션을 @Configuration이 붙은 설정 클래스에 붙임으로써 이와 관련된 기능들을 편리하게 제공하고 있다.
//어노테이션 기반의 SpringMvc를 구성할때 필요한 Bean 설정들을 자동으로 해주는 어노테이션이다.
//예)
//DispatcherServlet : SpringMVC의 핵심이며, 클라이언트의 HTTP요청을 적절한 컨트롤러로 보내주는 역할
//ContextLoaderListener : SpringApplicationContext를 생성하고 로드하는 역할을 한다. 
//						  어플리케이션 전반에 걸쳐 사용되는 Bean을 정의한 rootContext를 초기화 한다.
//ServletConfig : DispatcherServlet이 사용할 설정을 지정한다.
//RequestMappingHandlerMapping : @RequestMapping 어노테이션을 기반으로 URL과 컨트롤러 메서드를 매핑하는 역할을 하는 Bean
//RequestMappingHandlerAdapter : 컨트롤러 메서드의 실행을 처리하고, 반환값을 적절한 HTTP응답으로 변환하는 역할
//ViewResolver: 뷰의 논리적인 이름을 실제 뷰로 매핑합니다. 

@ComponentScan("컨트롤러 있는 패키지")
//@ComponentScan : @Component어노테이션 및 streotype(@Service, @Repository, @Controller)어노테이션이 부여된 Class들을
//자동으로 Scan하여 Bean으로 등록해주는 역할을 하는 어노테이션이다.
public class ServletContext implements WebMvcConfigurer {

	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		registry.addResourceHandler("/resources/**").addResourceLocations("/resources/");
	}

	@Bean
	public InternalResourceViewResolver resolver() {
		InternalResourceViewResolver resolver = new InternalResourceViewResolver();
		resolver.setViewClass(JstlView.class);
		resolver.setPrefix("/WEB-INF/views/");
		resolver.setSuffix(".jsp");
		return resolver;
	}
}

```

## vo패키지에 PersonVO클래스 만들기
```java
package vo;

import org.springframework.stereotype.Component;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class PersonVO {
	private String name,tel;
	private int age;
}

```
## non_spring.jsp 만들기
- 더이상 서블릿에서 내용을 가져오는게 아니기 때문에 스크립트릿을 사용해보자.
```jsp
<%@page import="vo.PersonVO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	PersonVO p1 = new PersonVO();
	p1.setName("일길동");
	p1.setAge(30);
	p1.setTel("010-111-2222");
	
	PersonVO p2 = new PersonVO("이길동","010-3333-3333",35);
	
	request.setAttribute("p1", p1);
	request.setAttribute("p2", p2);

%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<p>${ p1.name } / ${ p1.age } / ${ p1.tel }</p>
	<p>${ p2.name } / ${ p2.age } / ${ p2.tel }</p>
	
</body>
</html>

```

![image](image/person.png)

jsp까지는 구조가 복잡한 형태는 아니지만 스프링은 구동되는 순서가 다르면 이해하기 힘들다<br>

## PersonVO 코드 수정하기
```java
package vo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@Getter
public class PersonVO {

	private String name,tel;
	private int age;
	
	public PersonVO() {
		System.out.println("---PersonVO의 기본생성자---");
	}
	
	public PersonVO(String name, String tel, int age) {
		System.out.println("---파라미터를 받는 PersonVO 생성자 호출 ----");
		this.name = name;
		this.tel = tel;
		this.age = age;
	}

	
	public void setName(String name) {
		System.out.println("name setter호출");
		this.name = name;
	}

	
	public void setTel(String tel) {
		System.out.println("tel setter호출");
		this.tel = tel;
	}

	
	public void setAge(int age) {
		System.out.println("age setter호출");
		this.age = age;
	}
	
	
}
```

## RootContext.java 에 객체 생성하기
```java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
public class RootContext {

	//Bean
	//Spring에서 Bean은 어플리케이션의 핵심 구성 요소로 사용되는 객체를 말한다.
	//스프링은 제어의 역전(Inversion of Control, IoC)를 통해 객체의 생명주기와
	//의존성 관리를 스프링이 담당하게 된다.

	@Bean
	public PersonVO p1() {
		PersonVO p1 = new PersonVO();
		p1.setName("홍길동");
		p1.setTel("010-1111-1111");
		p1.setAge(30);
		return p1;
	}

	//이와 같이 객체를 생성하고 객체에 setter메서드에 값을 추가해주는것을 setter injection이라고 한다.
```
- 콘솔에 출력해보기

![image](image/setterInjection.png)

```java
	@Bean
	public PersonVO p2() {
		PersonVO p2 = new PersonVO("이길동","010-2222-2222",40);
		return p2;
	}

	//생성자에 값을 추가해주는것을 constructor injection이라고 한다.
}

```

실행해서 콘솔에서 출력문 뜨는지 확인하기

![image](image/constructorInjection.png)

- 스프링 bean은 기본이 싱글톤이기 때문에 메모리에 한번만 올린다.
- 이제 dao에 싱글톤 코드를 추가할 필요가 없어진다.
