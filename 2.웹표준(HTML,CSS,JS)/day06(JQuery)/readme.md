# JQuery란?
- 자바스크립트로 만들어진 라이브러리이다. 자바스크립트의 DOM(Document Object Model)작업을 좀 더 쉽게 할 수 있는 기능을 갖고 있다.

## JQuery Selectors
- Query 선택기는 이름, ID, 클래스, 유형, 속성, 속성 값 등을 기반으로 HTML 요소를 "찾기"(또는 선택)하는 데 사용됩니다.
- jQuery의 모든 선택자는 달러 기호와 괄호 $()로 시작합니다.

### 태그의 선택
- jQuery 요소 선택기는 요소 이름을 기반으로 요소를 선택합니다.
- 다음 <p>과 같이 페이지의 모든 요소를 선택할 수 있습니다 .
```
$("p")
```

### \#id 선택기
- jQuery 선택기는 HTML 태그의 id 속성을 사용하여 특정 요소를 찾습니다.
```
$("#test")
```

### .class 선택기
- jQuery .class선택기는 특정 클래스의 요소를 찾습니다.
```
$(".test")
```

## JQuery Events
- 웹 페이지가 응답할 수 있는 모든 다양한 방문자의 작업을 이벤트라고 합니다.

<table>
  <tr>
    <th>Mouse Events</th>
    <th>Keyboard Events</th>
    <th>Form Events</th>
    <th>Document/Window Events</th>
  </tr>
  <tr>
    <td>click</td>
    <td>keypress</td>
    <td>submit</td>
    <td>load</td>
  </tr>
  <tr>
    <td>mouseenter</td>
    <td>keyup</td>
    <td>focus</td>
    <td>scroll</td>
  </tr>
  <tr>
    <td>mouseleave</td>
    <td></td>
    <td>blur</td>
    <td>unload</td>
  </tr>
</table>
