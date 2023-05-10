## 의존성 주입

## Ex_날짜_SpringMVC
- com.korea.test_di

## dao 패키지에 BoardDAO 인터페이스 만들기

![image](https://github.com/to7485/Web1500/assets/54658614/467e3eac-259c-4d53-8549-56c25f04a188)

```
package dao;

import java.util.List;

public interface BoardDAO {

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
