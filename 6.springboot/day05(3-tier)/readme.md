# 3-tier
- Presentation Tier,Business Tier,Presistence Tier를 각각 물리적으로 독립된 모듈로 개발하고 유지하는 구조를 말한다.
- 스프링 프로젝트는 3-tier방식으로 구성한다.

![image](https://github.com/to7485/Web1500/assets/54658614/aef7e187-ffdb-4ab8-a287-b27b6ef368a9)

## tier의 종류

### Presentation Tier - 화면계층
- 화면에 보여주는 기술을 사용하는 영역
- Controller에서 사용자의 요청에 맞는 응답처리를 진행한다
- HTML엔진(Thymeleaf), HTML등이 담당하는 영역이다.
- 화면 구성이 이에 속한다.

### Business Tier - 비즈니스 계층
- 순수한 Business Logic을 담고있는 영역
- Presentation tier와 presistence tier의 bridge 역할을 한다.
- 고객이 원하는 요구사항을 반영하는 계층이다.
- 서비스에 있어서 가장 중요한 영역이다.
- 이 영역의 설계는 고객의 요구사항과 정확히 일치해야 한다.
- Service영역이다.(로그인, 마이페이지, 회원가입)
- 예를들어 쇼핑몰에서는 상품 구매시 포인트 적립을 가정한다면, Presistence tier의 설계는 '상품','회원'으로 나눠 설계하지만<br> Business tier는 상품 영역과 회원 영역을 동시에 사용해서 하나의 로직을 처리하게 된다.이 때 하나의 서비스에 필요한 여러 개의 쿼리 메서드를 하나로 묶어주는 역할이 필요한데, 이를 Service 객체로 사용한다.

### Presistence Tier - 영속 계층
- 데이터를 어떤 방식으로 보관하고, 사용하는 가에 대한 설계가 들어가는 계층
- 일반적으로 DBMS를 많이 사용한다.
- Presistence tier는 DBMS를 기준으로, Business tier는 Logic을 기준으로 처리한다.

각 영역은 독립적으로 설계되어 나중에 특정한 기술이 변하더라도 필요한 부분을 전자제품의 부품처럼 쉽게 교환할 수 있게 하자는 방식이다.<br> 각 연결부위는 인터페이스를 이용해서 설계하는 것이 일반적인 구성방식이다.

![image](https://github.com/to7485/Web1500/assets/54658614/c9fb3ccf-8cc0-4600-a0f8-9cce7d1b1cd4)

# 3-tier vs MVC

## 디자인 패턴
- 물릭적으로 구분할 수 없는 설계를 문서화 하면서 고안된 방법
- 프로그램의 규모가 커질수록 다양한 문제를 겪게 되는데, 이 문제들을 효율적으로 해결할 수 있게 유형별로 패턴화 한 것

## 차이점
- **3-tier 아키텍처** 는 물리적인 공간을 기준으로 역할을 구분
- **MVC 패턴**은 소프트웨어 디자인패턴 중 하나로 하나의 컴포넌트간 역할 분담

우리는 'MVC 패턴을 적용하였고 3-tier 구조로 되어있는 프레임워크를 갖추고 있다.'라고 할 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/ce86031e-de2c-46c5-a575-1337e92108dc)

# Ex_날짜_tier 프로젝트 생성하기
- application.properties 지우고 application.yml 파일 복사해오기
- com.korea.tier패키지 안에 vo,mapper,mybatis,controller 패키지 생성하기
  - 이전프로젝트에 있던 파일들 복사해오기
- src/main/resources 안에 mapper,config 패키지 생성하기
  - 이전프로젝트에 있던 파일들 복사해오기(경로만 잘 설정해주기)

![image](https://github.com/to7485/Web1500/assets/54658614/0ea7bb92-4f7e-45a8-b4e9-f21f18d5c2df)

## Order 테이블 생성하기

```sql
CREATE SEQUENCE SEQ_ORDER;

CREATE TABLE "ORDER"(
	ORDER_ID NUMBER PRIMARY KEY,
	PRODUCT_ID NUMBER NOT NULL,
	PRODUCT_COUNT NUMBER DEFAULT 1,
	ORDER_DATE DATE DEFAULT SYSDATE,
	CONSTRAINT FK_ORDER_PRODUCT FOREIGN KEY(PRODUCT_ID) REFERENCES PRODUCT(PRODUCT_ID)
);
```
## vo 패키지에 OrderVO 클래스 생성하기
```java
package com.korea.tier.vo;

import lombok.Data;

@Data
public class OrderVO {
	private int orderId;
	private int productId;
	private int productCount;
	private String orderDate;
}

```

## com.korea.tier패키지 안에 dao 패키지 생성하고 ProductDAO 클래스 생성하기

```java
package com.korea.tier.dao;

import java.util.List;

import org.springframework.stereotype.Repository;

import com.korea.tier.mapper.ProductMapper;
import com.korea.tier.vo.ProductVO;

import lombok.RequiredArgsConstructor;

@Repository
@RequiredArgsConstructor
public class ProductDAO {
	
	private final ProductMapper productMapper;
	
	//상품 추가
	public void save(ProductVO productVO) {
		productMapper.insert(productVO);
	}
	//상품 조회
	public List<ProductVO> findAll(){
		return productMapper.selectAll();
	}
}

```

Mapper -> DAO -> Service -> Controller 순으로 거쳐오면 된다.

## com.korea.tier패키지 안에 service 패키지 생성하고 ProductService 인터페이스 생성하기
- 지금은 컨트롤러 하나에 하나의 쿼리가 나가기 때문에 서비스에 대한 목적이 두드러지지 않는다.
- 여러개를 연동하다보면 하나의 서비스에 여러개의 쿼리가 필요하다 보니 묶어서 메서드 하나로 처리하기 위해 추가해주자.
- 예를들어 게시판을 구현하면 공지사항, 자유게시판, 후기게시판등 여러 게시판이 있을수 있다.
- 이때 공통적인 부분을 인터페이스로 추상화를 시켜놓고 사용하자는 것.(나중에 qualifier로 골라서 주입해주면 된다.)

```java
package com.korea.tier.service;

import java.util.List;

import com.korea.tier.vo.ProductVO;

public interface ProductService {
	
	//상품 추가
	public void register(ProductVO productVO);
	//상품 조회
	public List<ProductVO> getList(); 

}
```

## service 패키지에 ProductServiceImpl 클래스 생성하기
```java
package com.korea.tier.service;

import java.util.List;

import org.springframework.stereotype.Service;

import com.korea.tier.dao.ProductDAO;
import com.korea.tier.vo.ProductVO;

import lombok.RequiredArgsConstructor;

@Service
@RequiredArgsConstructor
public class ProductServiceImpl implements ProductService {
	
	private final ProductDAO productDAO;

	@Override
	public void register(ProductVO productVO) {
		productDAO.save(productVO);
		
	}

	@Override
	public List<ProductVO> getList() {
		
		return productDAO.findAll();
	}

}

```

## ProductController클래스 코드 수정하기
```java
package com.korea.tier.controller;

import java.util.List;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.view.RedirectView;

import com.korea.tier.mapper.ProductMapper;
import com.korea.tier.service.ProductService;
import com.korea.tier.vo.ProductVO;

import lombok.RequiredArgsConstructor;

@Controller
@RequiredArgsConstructor
@RequestMapping("/product/*")
public class ProductController { 

	//private final ProductMapper productMapper;
	//지금까지 서비스 하나당 쿼리문 하나라서 Mapper를 직접 주입했지만
	//매퍼를 여러개 추가하기 보다는 여러개를 묶어놓은 Service를 주입하자.
	
	private final ProductService productService;
	
	@GetMapping("register")
	public String register(Model model) {
		model.addAttribute("productVO",new ProductVO());
		return "product/product-insert";
	}
	
	@PostMapping("register")
	public RedirectView register(ProductVO productVO) {
		productService.register(productVO);
		return new RedirectView("product/list");
	}
	
	@GetMapping("list")
	public String list(Model model) {
		List<ProductVO> list = productService.getList();
		model.addAttribute("list",list);
		return "product/product-list";
	}
	
}
```

## templates에 product 폴더 생성하고 html파일들 복사해오기
### product-insert.html 코드 수정하기
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>상품 추가</title>
</head>
<body><!--action 수정하기-->
    <form th:action="@{/product/register}" th:object="${productVO}" method="post">
        <div>
            <input type="text" th:field="*{productName}" placeholder="상품 이름">
        </div>
        <div>
            <input type="text" th:field="*{productStock}" placeholder="상품 재고">
        </div>
        <div>
            <input type="text" th:field="*{productPrice}" placeholder="상품 가격">
        </div>
        <input type="submit" value="등록">
    </form>
</body>
</html>
```
- 실행하고 url에 localhost:1000/product/list 입력하여 결과 확인하기

![image](https://github.com/to7485/Web1500/assets/54658614/672f8094-7a02-4e57-93dc-917e459b40ec)

- localhost:10000/product/register 입력하여 추가가 잘 되는지도 확인하기

# 주문기능 만들기
- 제품 테이블과 PRODUCT_ID가 PK,FK로 연결이 되어 있다.
- 라디오 버튼을 만들고 주문할 개수를 입력할 수 있게 하고 버튼을 누르면 주문 테이블에 등록이 되게 만든다.
- 등록이 되는 동시에 제품 테이블에서는 재고가 감소하게 해야 한다.

## config.xml에 OrderVO의 별칭 만들어주기
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0/EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <typeAliases>
		<typeAlias type="com.korea.tier.vo.ProductVO" alias="productVO" />
		<typeAlias type="com.korea.tier.vo.OrderVO" alias="orderVO" />
	</typeAliases>
</configuration>
```

## mapper 패키지에 OrderMapper 인터페이스 만들기
```java
package com.korea.tier.mapper;

import org.apache.ibatis.annotations.Mapper;

import com.korea.tier.vo.OrderVO;

@Mapper
public interface OrderMapper {

	//주문하기
	public void insert(OrderVO orderVO);	
}
```

## dao 패키지에 OrderDAO 클래스 만들기
```java
package com.korea.tier.dao;

import org.springframework.stereotype.Repository;

import com.korea.tier.mapper.OrderMapper;
import com.korea.tier.vo.OrderVO;

import lombok.RequiredArgsConstructor;

@Repository
@RequiredArgsConstructor
public class OrderDAO {

	private final OrderMapper orderMapper;
	
	//주문하기
	public void save(OrderVO orderVO) {
		orderMapper.insert(orderVO);
	}
	
}
```
## service패키지에 OrderService 클래스 생성하기
- 나중에 음식만 팔다가 조리도구를 팔수도 있다.
- 인터페이스를 만들어놓고 클래스로 구현을 하는 이유는 확장을 염두해두는것.

```java
package com.korea.tier.service;

import org.springframework.stereotype.Service;

import com.korea.tier.dao.OrderDAO;
import com.korea.tier.vo.OrderVO;

import lombok.RequiredArgsConstructor;

@Service
@RequiredArgsConstructor
public class OrderService {

	private final OrderDAO orderDAO;
	
	//주문하기
	public void order(OrderVO orderVO) {
		orderDAO.save(orderVO);
	}
}
```

## mapper 폴더에 orderMapper.xml 파일 생성하기
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.korea.tier.mapper.OrderMapper">
	<insert id="insert">
		INSERT INTO ORDER (ORDER_ID, PRODUCT_ID, PRODUCT_COUNT)
		VALUES (SEQ_ORDER.NEXTVAL, #{productId}, #{productCount})
	</insert>
</mapper>
```

## controller 패키지에 OrderController 클래스 생성하기
```java
package com.korea.tier.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import com.korea.tier.service.OrderService;
import com.korea.tier.vo.OrderVO;

import lombok.RequiredArgsConstructor;

@Controller
@RequiredArgsConstructor
@RequestMapping("order/*")
public class OrderController {

	private final OrderService orderService;
	
	@GetMapping("order")
	public String order() {
		OrderVO vo = new OrderVO();
		vo.setProductId(4);
		vo.setProductCount(5);
		orderService.order(vo);
		return null;
		
	}
	
}
```
- 만약 실행을 한다면 order 테이블에 행이 추가될 것이다.

![image](https://github.com/to7485/Web1500/assets/54658614/dfee1a04-7510-4a44-b107-61afaaa32c01)

- 하지만 productCount(주문개수)는 늘지만 실제 product테이블에서의 재고도 update를 통해 감소하게 해줘야 한다.

## product.xml에 쿼리문 추가하기
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.korea.tier.mapper.ProductMapper">
	<insert id="insert">
		insert into product
		(PRODUCT_ID, PRODUCT_NAME, PRODUCT_STOCK, PRODUCT_PRICE)
		values (seq_product.nextVal, #{productName}, #{productStock}, #{productPrice})
	</insert>
	<select id="selectAll" resultType="productVO">
		select * from product
	</select>
	
	<update id="updateStock">
		UPDATE PRODUCT
		SET PRODUCT_STOCK = PRODUCT_STOCK - #{productCount} //주문한 개수만큼 재고 차감하기
		WHERE PRODUCT_ID = #{productId} //제품id에서
	</update>
</mapper>
```
## ProductMapper에 메서드 추가하기
```java
package com.korea.tier.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;

import com.korea.tier.vo.OrderVO;
import com.korea.tier.vo.ProductVO;

@Mapper
public interface ProductMapper {

	//상품 추가
	public void insert(ProductVO productVO);
	//상품 조회
	public List<ProductVO> selectAll();
	//상품 재고 수정
	public void updateStock(OrderVO orderVO);
}

```

## ProductDAO에 메서드 추가하기
```java
package com.korea.tier.dao;

import java.util.List;

import org.springframework.stereotype.Repository;

import com.korea.tier.mapper.ProductMapper;
import com.korea.tier.vo.OrderVO;
import com.korea.tier.vo.ProductVO;

import lombok.RequiredArgsConstructor;

@Repository
@RequiredArgsConstructor
public class ProductDAO {
	
	private final ProductMapper productMapper;
	
	//상품 추가
	public void save(ProductVO productVO) {
		productMapper.insert(productVO);
	}
	//상품 조회
	public List<ProductVO> findAll(){
		return productMapper.selectAll();
	}
	
	//상품 재고 수정
	public void setProductStock(OrderVO orderVO) {
		productMapper.updateStock(orderVO);
	}
}
```

## OrderService 클래스에서 메서드 사용하기
- 컨트롤러에서 dao를 두번 호출하는게 아니라 order 메서드 한번만 호출하면 DB에 두번 접근한다.
```java
package com.korea.tier.service;

import org.springframework.stereotype.Service;

import com.korea.tier.dao.OrderDAO;
import com.korea.tier.dao.ProductDAO;
import com.korea.tier.vo.OrderVO;

import lombok.RequiredArgsConstructor;

@Service
@RequiredArgsConstructor
public class OrderService {

	private final OrderDAO orderDAO;
	private final ProductDAO productDAO;
	
	//주문하기
	public void order(OrderVO orderVO) {
		productDAO.setProductStock(orderVO);
		orderDAO.save(orderVO);	
	}
}

```

![image](https://github.com/to7485/Web1500/assets/54658614/579774c0-a429-4ea8-92b7-0babe67f21a2)

- product 테이블에서도 재고가 같이 감소한 모습을 볼 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/73f42696-213f-4c10-ac09-6a38b0a39f1a)

# 사용자가 직접 선택하여 주문하게 하기

## OrderController에서 포워딩 해주기
```java
package com.korea.tier.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.view.RedirectView;

import com.korea.tier.service.OrderService;
import com.korea.tier.vo.OrderVO;

import lombok.RequiredArgsConstructor;

@Controller
@RequiredArgsConstructor
@RequestMapping("order/*")
public class OrderController {

	private final OrderService orderService;
	
	//주문
	@GetMapping("done")
	public RedirectView order(OrderVO orderVO) {
		System.out.println("주문개수 : " + orderVO.getProductCount());
		orderService.order(orderVO);
		return new RedirectView("/product/list");
		 
	}
	
}

```

## product-list.html 수정하기
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>상품 목록</title>
    <style>
        div {
            margin: 0 auto;
            width: 1000px;
        }

        table {
            width: 100%;
        }

        button {
            width: 50%;
        }
    </style>
</head>
<body>
    <div id="container">
        <table border="1">
            <tr>
                <th>단일 선택</th>
                <th>주문 개수</th>
                <th>상품 번호</th>
                <th>상품 이름</th>
                <th>상품 재고</th>
                <th>상품 가격</th>
                <th>등록 날짜</th>
                <th>수정 날짜</th>
            </tr>
            <th:block th:each="product : ${list}">
                <tr th:object="${product}">
                    <td><input type="radio" name="productId" th:value="*{productId}"></td>
                    <td><input type="text" class="productCount" readonly></td>
                    <td th:text="*{productId}"></td>
                    <td th:text="*{productName}"></td>
                    <td th:text="*{productStock}"></td>
                    <td th:text="*{productPrice}"></td>
                    <td th:text="*{registerDate}"></td>
                    <td th:text="*{updateDate}"></td>
                </tr>
            </th:block>
        </table>
        <button type="button" id="order-done">주문 완료</button>
    </div>
    <form th:action="@{/order/done}" method="get" name="order-form">
        <input type="hidden" name="productId">
        <input type="hidden" name="productCount">
    </form>
</body>
<script src="https://code.jquery.com/jquery-3.6.4.min.js"></script>
<script>
    const $radios = $("input[type='radio']"); //input태그중 type속성이 radio인 모든 요소 선택
    const $inputs = $("input[class='productCount']"); //input태그중 class 속성이 productCount인 모든 요소 선택
    const $done = $("#order-done"); //id속성이 order-form인 요소 선택
    const $form = $("form[name='order-form']"); //name 속성이 order-form인 form태그 선택
    let $temp, i; //temp는 임시로 사용할 변수, i는 선택된 라디오 버튼의 인덱스 저장

    $radios.on("click", function(){ //라디오버튼의 클릭 이벤트 처리하는 함수
       i = $radios.index(this); //i에 선택한 라디오 버튼의 index값을 저장
       if($temp) {
           $temp.prop("readOnly", true);
	//변수를 사용하여 이전에 선택된 입력 필드를 확인하고, readOnly 속성을 true로 설정하여 읽기 전용으로 변경합니다.
           $temp.val(""); 
       }
       $inputs.eq(i).prop("readOnly", false);
	//클릭된 라디오 버튼에 해당하는 입력 필드를 선택하고, readOnly 속성을 false로 설정하여 편집 가능한 상태로 변경합니다.
       $temp = $inputs.eq(i); //$temp 변수에 선택된 입력 필드를 저장합니다.
    });

    $done.on("click", function(){ //order-done 버튼의 클릭이벤트를 처리하는 함수
        if(i) {
            console.log(i);
            $form.find("input[name='productId']").val($radios.eq(i).val());
	    //폼 내에서 name 속성이 "productId"인 입력 필드를 선택하고, 선택된 라디오 버튼의 값을 설정합니다.
            $form.find("input[name='productCount']").val($inputs.eq(i).val());
	    //폼 내에서 name 속성이 "productCount"인 입력 필드를 선택하고, 선택된 입력 필드의 값을 설정합니다.
            console.log($form.find("input[name='productId']").val());
            console.log($form.find("input[name='productCount']").val());
            $form.submit();
	    폼을 제출합니다.
        }
    });
</script>
</html>
```
# 주문 내역 만들기
## product-list.html에 버튼 추가하기
```html
button {
	width: 50%;
	}
</style>
 ... 중략
<button type="button" id="order-done">주문 완료</button><button type="button" onclick="location.href='/order/list';">주문 내역</button>
```

![image](https://github.com/to7485/Web1500/assets/54658614/7d224395-04c3-4d4a-bf1d-c2be5f58dca9)

## order폴더에 order-list.html 파일 생성하기
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
	<head>
		<meta charset="UTF-8">
		<title>주문내역</title>
		<style>
				#container {
					width: 1000px;
					margin: 0 auto;
		
				}
				
				table{
					width:100%;
					
				}
		
				button {
					width: 100%;
				}
			</style>
	</head>
	<body>
		<div id="container">
			<table border="1">
					<tr>
						<th>상품 이름</th>
						<th>상품 가격</th>
						<th>주문 개수</th>
						<th>결제 금액</th>
						<th>주문 날짜</th>
					</tr>
					<th:block th:each="order : ${orders}">
						<tr th:object="${order}">
							<td th:text="*{productName}"></td>
							<td th:text="*{productPrice}"></td>
							<td th:text="*{productCount}"></td>
							<td th:text="*{orderPrice}"></td>
							<td th:text="*{orderDate}"></td>
						</tr>
					</th:block>
				</table>
				<button type="button" onclick="location.href='/product/list'">상품 목록</button>
			</div>
	</body>
</html>
```

## orderMapper.xml에 쿼리문 작성하기
- 현재 product 테이블과 order테이블은 productId컬럼을 통해 관계를 맺고 있다.
- product의 팔드와 order의 필드를 동시에 갖고 있는 vo가 없기 때문에 새롭게 만들어준다.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.korea.tier.mapper.OrderMapper">
	<insert id="insert">
		INSERT INTO "ORDER" (ORDER_ID, PRODUCT_ID, PRODUCT_COUNT)
		VALUES (SEQ_ORDER.NEXTVAL, #{productId}, #{productCount})
	</insert>
	
	<select id="selectAll" resultType="orderDTO">
		select P.PRODUCT_ID, PRODUCT_NAME, PRODUCT_STOCK, PRODUCT_PRICE, REGISTER_DATE, UPDATE_DATE, ORDER_ID, PRODUCT_COUNT, ORDER_DATE, PRODUCT_PRICE * PRODUCT_COUNT ORDER_PRICE
		FROM PRODUCT P JOIN ORDER O ON P.PRODUCT_ID = O.PRODUCT_ID
	</select>
</mapper>
```

## vo 패키지에 OrderDTO만들기
```java
package com.korea.tier.vo;

import lombok.Data;

@Data
public class OrderDTO {
	
	private int productId;
	private String productName;
	private int productStock;
	private int productPrice;
	private String registerDate;
	private String updateDate;
	private int orderId;
	private int productCount;
	private String orderDate;
	private int orderPrice;

}
```

## config.xml에 별칭 만들어주기
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0/EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <typeAliases>
		<typeAlias type="com.korea.tier.vo.ProductVO" alias="productVO" />
		<typeAlias type="com.korea.tier.vo.OrderVO" alias="orderVO" />
		<typeAlias type="com.korea.tier.vo.OrderDTO" alias="orderDTO" />
	</typeAliases>
</configuration>
```

## OrderMapper인터페이스에 메서드 생성하기
```java
package com.korea.tier.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;

import com.korea.tier.vo.OrderDTO;
import com.korea.tier.vo.OrderVO;

@Mapper
public interface OrderMapper {

	//주문하기
	public void insert(OrderVO orderVO);
	
	//주문내역
	public List<OrderDTO> selectAll();
}

```

## dao패키지의 OrderDAO클래스에 메서드 추가하기
```java
package com.korea.tier.dao;

import java.util.List;

import org.springframework.stereotype.Repository;

import com.korea.tier.mapper.OrderMapper;
import com.korea.tier.vo.OrderDTO;
import com.korea.tier.vo.OrderVO;

import lombok.RequiredArgsConstructor;

@Repository
@RequiredArgsConstructor
public class OrderDAO {

	private final OrderMapper orderMapper;
	
	//주문하기
	public void save(OrderVO orderVO) {
		orderMapper.insert(orderVO);
	}
	
	//주문내역
	public List<OrderDTO> findAll(){
		return orderMapper.selectAll();
	}
	
}

```

## OrderService에 메서드 추가하기
```java
package com.korea.tier.service;

import java.util.List;

import org.springframework.stereotype.Service;

import com.korea.tier.dao.OrderDAO;
import com.korea.tier.dao.ProductDAO;
import com.korea.tier.vo.OrderDTO;
import com.korea.tier.vo.OrderVO;

import lombok.RequiredArgsConstructor;

@Service
@RequiredArgsConstructor
public class OrderService {

	private final OrderDAO orderDAO;
	private final ProductDAO productDAO;
	
	//주문하기
	public void order(OrderVO orderVO) {
		orderDAO.save(orderVO);
		productDAO.setProductStock(orderVO);
	}
	
	//주문내역
	public List<OrderDTO> getList(){
		return orderDAO.findAll();
	}
}
```

## OrderController에 코드 추가하기
```java
package com.korea.tier.controller;

import java.util.List;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.view.RedirectView;

import com.korea.tier.service.OrderService;
import com.korea.tier.vo.OrderDTO;
import com.korea.tier.vo.OrderVO;

import lombok.RequiredArgsConstructor;

@Controller
@RequiredArgsConstructor
@RequestMapping("order/*")
public class OrderController {

	private final OrderService orderService;
	
	//주문
	@GetMapping("done")
	public RedirectView order(OrderVO orderVO) {
		System.out.println("주문개수 : " + orderVO.getProductCount());
		orderService.order(orderVO);
		return new RedirectView("/product/list");
		 
	}
	
	@GetMapping("list")
	public String list(Model model) {
		List<OrderDTO> list = orderService.getList();
		model.addAttribute("orders",list);
		return "/order/order-list";
		
	}
	
}

```
## 실행하여 확인하기

![image](https://github.com/to7485/Web1500/assets/54658614/5e634197-dab3-472b-aa07-ef81752a4c37)



