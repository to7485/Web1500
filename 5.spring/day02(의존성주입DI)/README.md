## 의존성 주입

## Ex_날짜_SpringMVC
- com.korea.test_di

## dao 패키지에 BoardDAO 인터페이스 만들기

![image](https://github.com/to7485/Web1500/assets/54658614/467e3eac-259c-4d53-8549-56c25f04a188)

```
package dao;

import java.util.List;

public interface BoardDAO {

	//Dao(Data Access Object)는 기본적으로 CRUD기능을 가지고 있다.
	//나중에 사용할 추상메서드를 준비해보자.
	//파라미터를 받을 수도 있긴 한데 뭘 받을지 모르기 때문에 모든걸 다 받을 수 있는 		
	//Object타입으로 만들자!

	int insert( Object  ob);
	
	List<Object> selectList();
	
	int update(Object ob);
	
	int delete(int idx);
}
인터페이스만 가지고는 아무것도 못한다.
인터페이스를 구현하는 자식클래스를 만들어주자.
```
## dao 패키지에 BoardDAOImpl 클래스 만들기
```
package dao;

import java.util.List;

public class BoardDAOImpl implements BoardDAO {

	//원래 DAO는 DB에 연결하여 List를 가지고 오는 작업을 하지만
	//이 예제에서는 그 부분은 생략했다.

	@Override
	public int insert(Object ob) {
		// TODO Auto-generated method stub
		return 0;
	}

	DB에서 값이 넘어왔다고 가정하고 리스트에 담아서 넘겨보기
	@Override
	public List<Object> selectList() {
		List<Object> list = new ArrayList<Object>();
		list.add("참외");
		list.add("사과");
		list.add("수박");
		list.add("복숭아");
		return list;
	}
	
	//얘가 dao 이니까 servlet이 받아서 바인딩하고 포워딩을 해서 jsp에 출력을 해줘야 된다.
	//그 사이에 service라고 하는애가 받아준다.

	@Override
	public int update(Object ob) {
		// TODO Auto-generated method stub
		return 0;
	}

	@Override
	public int delete(int idx) {
		// TODO Auto-generated method stub
		return 0;
	}

}

```

## service 패키지에 BoardService 인터페이스 만들기
```
package service;

import java.util.List;

public interface BoardService {

	List<Object> selectList();
	
}
```

## 인터페이스를 구현할 BoardServiceImpl 클래스 만들기
```
package service;

import java.util.List;

import dao.BoardDAOImpl;

public class BoardServiceImpl implements BoardService {

	BoardDAOImpl dao;
	
	public BoardServiceImpl(BoardDAOImpl dao) {
		this.dao = dao;
	}
	
	@Override
	public List<Object> selectList() {
		
		return dao.selectList();
	}

}

```

![image](https://github.com/to7485/Web1500/assets/54658614/eddf47a4-8c04-4287-a336-5f1d91034078)

## bean 객체를 생성해보자
```
<!-- BoardDaoImpl dao = new BoardDaoImpl();-->
	<bean id="dao" class="dao.BoardDaoImpl"></bean>	
	
	<!-- BoardServiceImpl service = new BoardServiceImpl(dao); -->
	<bean id="serviceBean" class="service.BoardServiceImpl">
	
		<!-- 생성자 파라미터로 dao를 받아준다!! -->
		<constructor-arg ref="dao"/>
	<!-- ref를 통해 다른 bean객체를 참조하는 형태를 Dependency injection(의존성 주입)이라고 한다. -->
	</bean>
</beans>
```

## BoardController 클래스 생성하기
```
//↓↓↓ 스프링으로 하여금 해당클래스가 Controller라는 것을 인식시키기 위해
//꼬리표(어노테이션 :프로그램 주석)이 "반드시" 필요하다.

@Controller    
public class BoardController {
	//root-context.xml은 Controller를 제외한 모든 객체를 만든다.
	//유일하게 컨트롤러만이 servlet-context.xml에서 만들어진다.
	BoardServiceImpl service;
	
	public BoardController(BoardServiceImpl service;) {
		System.out.println("--BoardController()의 생성자--");
		this.service = service;
	}

	public void setService(BoardServiceImpl service) {
		//셋터인젝션을 사용하기 위한 셋터메서드 생성
		this.service = service;
	}
	
}
```

## servlet-context.xml에 컨트롤러 객체 만들기
```
	...............
	
	         <!-- 오류 방지를 위해 controller패키지는 지워주자 ↓↓↓↓-->
	<!-- BoardController생성 -->
	<!-- 자동생성(오토디텍팅)과 수동생성은 동시에 정의하면 오류가 난다. -->
	<context:component-scan base-package="com.increpas.test_di" />
	
	<!-- BoardController수동생성 -->
	<!-- 자동완성을 사용하는 경우 생성자 파라미터나 setter 메서드를 호출하는 것이 불가능 하므로 생성자나
	stter로 받아야 할 데이터가 있다면 자동이 아닌 수동으로 컨트롤러를 생성해줘야 한다. -->
	
	<beans:bean class="com.korea.test_di.BoardController">
	<!-- 생성자 인젝션을 통해 컨트롤러에 값 추가하기 -->
		<beans:constructor-arg ref=“serviceBean”/>
	
		<!-- name="service" : BoardController의 setService( service ) 메서드의 파라미터
		     ref="serviceBean" : root-context에서 만든 id -->
	<!-- setter 인젝션을 통해 컨트롤러에 값 추가하기 -->
		<beans:property name="service" ref="serviceBean"></beans:property>
	</beans:bean>
	
</beans:beans>
```
