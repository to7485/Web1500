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
	
	$registerDone.on("click",function(){
		$(this).hide();
		$registerReady.show();
		
	});

    $radios.on("click", function(){
       i = $radios.index(this);
       if($temp) {
           $temp.prop("readOnly", true);
           $temp.val("");
       }
       $inputs.eq(i).prop("readOnly", false);
       $temp = $inputs.eq(i);
    });

    $done.on("click", function(){
        if(i) {
            console.log(i);
            $form.find("input[name='productId']").val($radios.eq(i).val());
            $form.find("input[name='productCount']").val($inputs.eq(i).val());
            console.log($form.find("input[name='productId']").val());
            console.log($form.find("input[name='productCount']").val());
            $form.submit();
        }
    });
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

# 주문하기 기능 만들기

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
		url: "new",
		type:"post",
		data:JSON.stringify({productName:$("#productName").val(), productStock:$("#productStock").val(), productPrice:$("#productPrice").val()}),
		contentType:"application/json; charset=utf-8",
		success: function(){
			location.reload();//현재 페이지 새로고침
		}
	});
});

...

```
