# REST(Representational State Transfer)
- 웹에서 데이터를 전송하고 처리하는 방법을 정의한 인터페이스를 말한다.
- 데이터 전송 시 이름으로 상태를 알 수 있도록 전송하는 방식
- 예) 101번 상품의 정보를 전달하고 싶을 때 URL에 -> product/101 을 알 수 있다.

## REST의 개념
- HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,<br> HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한<br> CRUD Operation을 적용하는 것을 의미한다.
- REST는 자원 기반의 구조(ROA, Resource Oriented Architecture) 설계의 중심에 Resource가 있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐를 의미한다.
- 웹 사이트의 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 ID인 HTTP URI를 부여한다.

※ IP주소부터 끝까지가 URL이다. IP를 제외한 주소를 URI라고 한다.

# Ex_날짜_REST 프로젝트 생성하기
- 페이지 이동 없이 하나의 페이지에서 모든 데이터를 주고받아보자
- resources 에 config, mapper 패키지, application.yml 복사하기(mapper.xml파일,config.xml 에 mapper와 경로 맞춰주자)
- java에 mapper,mybatis,vo,dao,service 옮기기

## controller 패키지 생성하고 ProductController 클래스 생성하기
```java
package com.korea.rest.controller;

import org.springframework.stereotype.Controller;

@Controller
public class ProductController {

}

```

## templates에 product.html 생성하기
- tier 프로젝트의 product-list.html의 내용을 복사해오고 수정하기
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
  
  		button.register-ready {
  			width: 100%
  		}
  		
  		button.register-done {
              		width: 100%;
          	}
  
  		div.register-wrap {
  			display: none;
  			width: 500px;
  		}
  
  		div.register-wrap div {
  			width: 100%;
  		}
  		
  		 div.register-wrap input {
              		width: 100%;
          	}
  	</style>
</head>
<body>
    <div>
		<button type="button" class="register-ready">상품 추가</button>
		<div class="register-wrap" th:object="${productForm}">
			 <div>
	            <input type="text" th:field="*{productName}" placeholder="상품 이름">
	        </div>
	        <div>
	            <input type="text" th:field="*{productStock}" placeholder="상품 재고">
	        </div>
	        <div>
	            <input type="text" th:field="*{productPrice}" placeholder="상품 가격">
	        </div>
	        <input type="button" class="register-done" value="등록">
        </div>
        
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
        <button type="button" id="order-done">주문 완료</button><button type="button" onclick="location.href='/order/list';">주문 내역</button>
    </div>
    <form th:action="@{/order/done}" method="get" name="order-form">
        <input type="hidden" name="productId">
        <input type="hidden" name="productCount">
    </form>
    
    
    
</body>
<script src="https://code.jquery.com/jquery-3.6.4.min.js"></script>
<script>
    const $radios = $("input[type='radio']");
    const $inputs = $("input[class='productCount']");
    const $done = $("#order-done");
    const $form = $("form[name='order-form']");
    const $registerReady = $("button.register-ready");
    const $registerDone = $("button.register-done");
    let $temp, i;
    
	$registerReady.on("click",function(){
		
		$(this).hide();
		$("div.register-wrap").show();
	
	});
	
	$registerDone.on("click", function(){
	
		$("div.register-wrap").hide();
		$registerReady.show();
	});

...

</script>
</html>
```

## ProductController에서 매핑 잡아주기
```java
@Controller
@RequiredArgsConstructor
public class ProductController {
	
	private final ProductService productService;
	
	@GetMapping("list")
	public String list(Model model) {
		
		List<ProductVO> list = productService.getList();
		model.addAttribute("productForm",new ProductVO());
		model.addAttribute("list",list);
		return "product";
	}
}

```
## 실행하고 확인하기

![image](https://github.com/to7485/Web1500/assets/54658614/48d00bc8-97b0-4c59-85a4-39550a140d44)

# 등록하기 기능 만들기

## ProductController 코드 추가하기
```java
@PostMapping("new")
@ResponseBody //JSON의 키값이 VO의 필드로 잡힌다			
public void register(@RequestBody ProductVO productVO) {
	productService.register(productVO);
}
```

## product.xml 코드 수정하기
```js
...

$registerDone.on("click", function () {
	/*$(this).hide();
	$registerReady.show();
	등록버튼 누르면 새로고침이 될것이기 때문에 필요없다.
	*/
	$.ajax({
		url: "new", //URL 요청
		type:"post", //전송 방식
		data:JSON.stringify({productName:$("#productName").val(), productStock:$("#productStock").val(), productPrice:$("#productPrice").val()}),
		//요청과 함께 전송할 데이터를 설정합니다. 이 경우 데이터는 객체 형태이며,JSON.stringify()를 사용하여 먼저 JSON 문자열로 변환됩니다.
		contentType:"application/json; charset=utf-8", //요청의 콘텐츠 타입을 JSON으로 설정합니다.
		success: function(){ //성공적으로 처리된 경우 실행될 성공 콜백 함수입니다. 이 경우 location.reload()를 사용하여 현재 페이지를 새로 고침합니다. 
			location.reload();
		}
	});
});

...

```
- 실행하면 제품이 추가 되는걸 확인할 수 있다.

# 주문내역 띄우기

## 주문내역 버튼을 눌렀을 때 밑에 띄워주기

## product.html 코드 수정하기
```html

스타일 수정 
span {
    cursor: pointer;
}

span.on {
    font-weight: bold;
}

#container {
    margin: 0 auto;
    width: 1000px;
}

div.sort{
	text-align: right;
}

코드 수정

<button type="button" id="order-done">주문 완료</button><button type="button" id="order-list">주문 내역</button>
</div>

tier 프로젝트에 order-list.xml에 div부분 복사해오기

<div id="container">
	<div class="sort">
	    <span class="on" id="recent" data-sort="recent">최신순</span>
	    <span class="" id="money" data-sort="money">결제 금액순</span>
	</div>
----------------------------------------테이블 부분은 JS로 들어가야 한다.
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
----------------------------------------------------------------------
</div>
```
- 주문 내역 버튼을 눌렀을 때 ajax를 이용해 아래에 주문내역 테이블이 뜨도록 코드 작성하기

## OrderController 생성하기
```java
@RestController
@RequiredArgsConstructor
@RequestMapping("/order/*")
public class OrderController {

    private final OrderService orderService;
	
    @GetMapping("list/{sort}")
    //@PathVariable : 경로 변수를 메서드의 매개변수로 바인딩하는 데 사용
    public List<OrderDTO> list(@PathVariable("sort") String sort){
        return orderService.getList(sort);
    }
}
```
### @RestController
- @RestController는 Restful Web API를 좀 더 쉽게 만들기 위해 스프링 프레임워크 4.0에 도입된 기능이다.
- @Controller와 @ResponseBody를 합쳐놓은 어노테이션이다.
- 클래스 이름 위에 @Controller 어노테이션을 선언하면 해당 클래스의 요청을 처리하는 컨트롤러로 사용한다.
- @ResponseBody 어노테이션은 자바 객체를 HTTP응답 본문 객체로 변환해 클라이언트에 전송한다.


## product.html 코드 작성하기
- 주문내역 버튼을 눌렀을 때만 테이블 보이게 하기
```html
<div id="container">
	<div class="sort"> 
		<span class="on" id="recent" data-sort="recent">최신순</span>
		<span class="" id="money" data-sort="money">결제 금액순</span>
	</div>
	<div class="order-list"></div>
</div>

... 중략
let $temp, i, sort; //sort 변수 정의
const $spans = $("div.sort span");//div 요소 내부에 있는 class 속성 값이 "sort"인 모든 span 요소들을 선택
const $orderList = $("button#order-list");

$spans.on("click", function () {
		$spans.attr("class", ""); //전체 span 태그의 class를 비우고
		$(this).attr("class", "on"); //누른애만 class의 값을 on으로 바꾼다.
		$orderList.click(); //버튼을 누른다.
});

$orderList.on("click", function () { //주문내역 버튼을 눌렀을 때
	$("#container").show(); //주문내역 코드가 보이게 하기
	$spans.each((i, span) => {
		if ($(span).attr("class")) {
			sort = $(span).data("sort");
		}
	});
//$spans.each() 메서드를 사용하여 $spans 객체의 각 span 요소에 대해 반복문을 실행합니다.
// 반복문은 인덱스 i와 현재 요소 span을 매개변수로 받는 콜백 함수를 실행합니다.
//콜백 함수 내에서는 조건문을 사용하여 현재 span 요소의 class 속성을 확인합니다.
// $(span).attr("class")를 통해 span 요소의 class 속성 값을 가져옵니다.
//만약 class 속성이 존재한다면, $(span).data("sort")를 사용하여 span 요소의 sort 데이터 속성 값을 가져옵니다.
// 이 값을 sort 변수에 할당합니다.

	$("span").attr("class", "");
	$("span#" + sort).attr("class", "on");
	$.ajax({
		url: "/order/" + sort,
		success: function (orders) {
			let text = `
	    <table border="1">
		<tr>
		    <th>상품 이름</th>
		    <th>상품 가격</th>
		    <th>주문 개수</th>
		    <th>결제 금액</th>
		    <th>주문 날짜</th>
		</tr>
	`;
	orders.forEach(order => {
		text += `
		    <tr>
			<td>${order.productName}</td>
			<td>${order.productPrice}</td>
			<td>${order.productCount}</td>
			<td>${order.orderPrice}</td>
			<td>${order.orderDate}</td>
		    </tr>
	    `;
	});
	text += `</table>`;

	$("div.order-list").html(text);
		}
	});
});
```

# 주문완료 처리하기
- 주문하면서 재고 감소하게 하기

## OrderController에 매핑 추가하기
```java
//주문하기
@PostMapping("write")
public void register(@RequestBody OrderVO orderVO){
orderService.order(orderVO);
}
```

## product.html 코드 추가하기
```html

<body>
    <div>
        <button type="button" class="register-ready">상품 추가</button>
        <div class="register-wrap" th:object="${productForm}">
            <div>
                <input type="text" th:field="*{productName}" placeholder="상품 이름">
            </div>
            <div>
                <input type="text" th:field="*{productStock}" placeholder="상품 재고">
            </div>
            <div>
                <input type="text" th:field="*{productPrice}" placeholder="상품 가격">
            </div>
            <input type="button" class="register-done" value="등록">
        </div>
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
        <button type="button" id="order-done">주문 완료</button><button type="button" id="order-list">주문 내역</button>
    </div>
    <div id="container">
        <div class="sort">
            <span class="on" id="recent" data-sort="recent">최신순</span>
            <span class="" id="money" data-sort="money">결제 금액순</span>
        </div>
        <div class="order-list"></div>
    </div>
</body>
<script src="https://code.jquery.com/jquery-3.6.4.min.js"></script>
<script>
    const $radios = $("input[type='radio']");
    const $inputs = $("input.productCount");
    const $done = $("#order-done");
    const $form = $("form[name='order-form']");
    const $registerReady = $("button.register-ready");
    const $registerDone = $("input.register-done");
    const $orderList = $("button#order-list");
    let $temp, i, sort;
    const $spans = $("div.sort span");

    const $ids = $("input[name='productId']");

    $done.on("click", function(){ //주문완료 버튼을 눌렀을 때
        $.ajax({
            url: "/order/write",
            type: "post",
            data: JSON.stringify({productId: $ids.eq(i).val(), productCount: $inputs.eq(i).val()}),
            contentType: "application/json; charset=utf-8",
            success: function(){ //주문이 성공했을 때
----------------------------아래 컨트롤러 매핑 작성하고 돌아오기--------------------------------------------
                $.ajax({ //재고 감소를 위한 ajax
                    url: "/product/" + $ids.eq(i).val(),
                    success: function(products) {
                        console.log(products.productStock);
                        $($("tr").eq(i + 1).children()).eq(4).text(products.productStock);
                    }
                });
----------------------------아래 컨트롤러 매핑 작성하고 돌아오기--------------------------------------------
                $orderList.click();
            }
        });
    });


</script>
</html>
```

## ProductController에 매핑 추가하기
```java
@GetMapping("{productId}")
@ResponseBody
public ProductVO getProduct(@PathVariable("productId") int productId){
return productService.getProduct(productId);
}
```

## productMapper.xml 쿼리문 추가하기
```xml
<select id="select" resultType="productVO">
	select * from product where PRODUCT_ID = #{productId}
</select>
```
## ProductMapper 인터페이스에 추가하기
```java
//상품 조회
public ProductVO select(int productId);
```

## ProductDAO에 메서드 추가하기
```java
//상품조회
public ProductVO findById(int productId) {
	return productMapper.select(productId);
}
```

## ProductService에 메서드 추가하기
```java
//상품 조회
public ProductVO getProduct(int productId);
```

## ProductServiceImpl에 메서드 추가하기
```java
@Override
public ProductVO getProduct(int productId) {
	// TODO Auto-generated method stub
	return productDAO.findById(productId);
}
```
