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

![image](https://github.com/to7485/Web1500/assets/54658614/fdb93ac4-ae99-4d4c-876d-47b51eb9852f)
