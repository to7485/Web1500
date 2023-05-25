# 스프링 AOP
- Aspect-Oriented Programming(관점지향 프로그래밍)

자바의 객체지향 프로그래밍을 보완하는 개념.<br>
어플리케이션을 객체지향적으로 모듈화 하여 작성하더라도 다수의 객체들에 분산되어 중복적으로 존재하는 공통관심사가 여전히 존재한다.<br>

![image](https://github.com/to7485/Web1500/assets/54658614/1a3612d1-8713-49d7-a0b0-93f6574437fd)

여러 객체에 공통적으로 적용할 수 있는 기능을 분리해 재사용성을 높여주는 기법<br>

## 특징
- 공통 기능 구현과 핵심 기능 구현의 분리
- 핵심 기능에 공통 기능을 삽입하며 다음과 같은 방법이 존재
  - 런타임에 프록시 객체를 생성해서 공통 기능을 삽입
- 스프링 AOP는 프록시 객체를 자동으로 생성해줌
- AOP의 공통 기능을 Aspect라 칭하며, 아래와 같은 주요 용어가 존재
  - Target
    - 어떤 대상에 부가 기능을 부여할 것인가.
  - Advice
    - 언제,어떤 부가기능? Before, AfterReturning,AfterThrowing, After,Around
  - Joinpoint
    -  Advice를 적용 가능한 지점을 의미(어디에 적용할 것인가? 메서드, 필드, 객체, 생성자 등)
  - Pointcut
    - 실제 advice가 적용될 지점, SpringAOP에서는 advice가 적용될 메서드를 선정
  - Weaving
    - Advice를 핵심 로직 코드에 적용하는 것
  - Aspect
    - 여러 객체에 공통으로 적용되는 기능

## 프록시 패턴
- 컴퓨터 프로그래밍 소프트웨어 디자인 패턴의 하나
- 다른 무언가와 이어지는 인터페이스 역할을 하는 클래스
- 어떤 객체에 대한 접근을 제어하는 용도로 대리인이나 대변인에 해댕하는 객체를 제공하는 패턴


## Advice의 종류
- before Advice : 대상 객체의 메서드 호출 전에 공통 기능을 실행
- After Returning Advice : 대상 객체의 매서드가 익셉션 없이 실행된 이후에 공통 기능을 실행
- After Throwing Advice: 대상 객체의 매서드를 실행하는 도중 익셉션이 발생한 경우에 공통 기능을 실행
- After Advice: 익셉션 발생 여부에 상관없이 대상 객체의 매서드 실핼 후 공통 기능을 실행
- Around Advice: 대상 객체의 매서드 실행 전, 후 또는 익셉션 발생 시점에 공통 기능을 실행
## Ex_날짜_AOP 프로젝트 생성
- pom.xml, resources의 패키지 옮기기


# 리스트에 사람을 저장하고 출력하는 기능 만들어보기
- 핵심기능 : 리스트에 추가, 호출
## emp 패키지 생성후 Emp.java 클래스만들기
```java
package xml;

import lombok.Getter;
import lombok.Setter;

@Setter
@Getter

public class Emp {
	private String empno;
	private String ename;
	
	public Emp(String empno,String ename) {
		this.empno = empno;
		this.ename = ename;
	}
	
	@Override
	public String toString() {
		// TODO Auto-generated method stub
		return "Emp [empno="+empno+", ename="+ ename + "]";
	}
}

```

## 추가와 출력 기능이 있는 EmpManager클래스 생성하기
```
package xml;

import java.util.ArrayList;
import java.util.List;

public class EmpManager {
	
	List<Emp> emps = new ArrayList<Emp>();

	
	public List<Emp> getAllEmps() throws Exception {
		System.out.println("[Target Method] getAllEmps() called...");
		return emps;
	}

	public void setEmp(Emp e) {
		emps.add(e);	
	}

}

```

## EmpLoggingAspect클래스 만들기
```
package xml;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.context.annotation.Configuration;



@Aspect
public class EmpLoggingAspect {
	
  //@Before 어노테이션을 사용하며 포인트컷 메소드가 실행되기 전에 충고가 적용된다.
  //* emp.EmpManager.get*(..) : emp 패키지에 EmpManager클래스의 get으로 시작하는 메서드에 파라미터의 개수는 상관없다는 뜻          
	@Before("execution(* emp.EmpManager.get*(..))")
	public void before(JoinPoint joinPoint) {
  //joinPoint.getSignature() : advice되는 메서드에 대한 정보를 반환
  //getName() : 이름을 반환
		System.out.println(">>>>>>>>>> Before Advice : " + joinPoint.getSignature().getName());
	}
	
  //@Around 어노테이션을 사용하며 포인트컷 메소드가 실행 되기 전, 리턴 된 후에 충고가 적용된다.
  //어드바이스 메소드의 첫번째 파라미터는 ProceedingJoinPoint가 되어야 한다. proceed() 메소드를 통해 타겟 클래스의 원래 메소드를 호출
	@Around("execution(* emp.EmpManager.get*(..))")
	public Object around(ProceedingJoinPoint pjp) throws Throwable{
    //
		System.out.println(">>>>>>>>>> Around Advice[전] : " + pjp.getSignature().getName());
		
		Object o = pjp.proceed();
		
		System.out.println(">>>>>>>>>> Around Advice[후] : " + pjp.getSignature().getName());
		return o;
	}
	
  //@AfterReturning 어노테이션을 사용하며 포인트컷 메소드가 리턴(정상종료)된 후 충고가 적용된다.
	@AfterReturning(pointcut = "execution(* emp.EmpManager.*(..))", returning = "retVal")
	public void afterReturning(JoinPoint joinPoint, Object retVal) {
		System.out.println(">>>>>>>>>>>>> AfterReturning Advice : " + joinPoint.getSignature().getName());
		System.out.println(">>>>>>>>>>>>> AfterReturning Advice return value is " + retVal);
	}
	
  //@After 어노테이션을 사용한다. 포인트컷 메소드가 실행된 후(정상종료 여부와 관계없이) 충고가 적용된다.
	@After("execution(* xml.EmpManagerImpl.get*(..))")
	public void after(JoinPoint joinPoint) {
		System.out.println(">>>>>>>>>>>>> After Advice : " + joinPoint.getSignature().getName());
	}
	
  //@AfterThrowing 어노테이션을 사용하며 포인트컷 메소드에서 예외가 발생할 때 충고가 적용된다.
	@AfterThrowing(pointcut = "execution(* emp.EmpManager.*(..))", throwing="ex")
	public void logAfterThrowingAllMethods(Exception ex) throws Throwable {
		System.out.println(">>>>>>>>>>>>> @AfterThrowing Advice : " + ex);
	}
	
}

```

## context패키지에 Context_5_AOP 클래스 만들기
```
package context;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

import xml.EmpLoggingAspect;
import xml.EmpManagerImpl;



@Configuration
@EnableAspectJAutoProxy
public class Context_5_aop {
	
	@Bean
	public EmpManagerImpl empManager() {
		return new EmpManagerImpl();
	}
	
	@Bean
	public EmpLoggingAspect loggingAspect() {
		return new EmpLoggingAspect();
	}
	
}

```

## emp 패키지에 EmpTest클래스 만들기
```
package xml;

import java.util.List;

import org.aspectj.apache.bcel.util.ClassPath;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import context.Context_5_aop;

public class EmpTest {
	public static void main(String[] args) throws Exception {
		ApplicationContext context = new AnnotationConfigApplicationContext(Context_5_aop.class);
		EmpManagerImpl manager = (EmpManagerImpl) context.getBean("empManager");
		
		manager.setEmp(new Emp("1","홍길동"));
		manager.setEmp(new Emp("2","이종철"));
		
		List<Emp> emps = manager.getAllEmps();
		System.out.println(emps);
	}
}

```






