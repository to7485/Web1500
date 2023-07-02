# 게시판 만들기

## Ex_날짜_app 프로젝트 생성

<img width="155" alt="image" src="https://github.com/to7485/Web1500/assets/54658614/d3388ad6-1557-4127-be22-3dd0d49f39ef">

- resoucres 패키지에 config,mapper 패키지 생성하고 파일 복사하기(mapper.xml파일은 하나만 가져와도 된다.), yml파일 복사해서 넣기
- java 패키지에 controller, mapper, service, domain패키지 생성하기 (domain 패키지 안에 dao, dto, vo패키지 만들기)

<img width="190" alt="스크린샷 2023-07-02 오후 3 48 08" src="https://github.com/to7485/Web1500/assets/54658614/cc164779-024d-45ce-9b4a-472dba37382c">


## config.xml 파일에 alias 지우기
```xml
<?xml version="1.0" encoding="UTF-8"?>
	  
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0/EN" "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

## orderMapper.xml 쿼리문 지우기
```xml
<?xml version="1.0" encoding="UTF-8"?>
	  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0/EN" "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.korea.rest.mapper.OrderMapper">

</mapper>
```

## static 패키지 안에 업로드한 파일들 넣어주기
- images, js, css, 폰트 등등이 들어있는 폴더 채로 넣어주자.

<img width="122" alt="image" src="https://github.com/to7485/Web1500/assets/54658614/9fdf9101-66b2-4777-b5d2-5bb105486f56">


## tamplates 패키지에 board,error패키지 생성하기

## error패키지에 404.html,500.html파일 생성하기
```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    
    <!-- IE에서도 잘 나오게 하는 코드 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
     
     
    <!-- viewport : 모바일웹이나 반응형웹에서 각각의 기기장치를 인식할 때 사용하는 태
		width=device-width 장치의 화면 너비를 따르도록 페이지 너비를 설정
    	initial-scale=1 브라우저에서 처음 로드할 때 초기 확대/축소 수준 설--> 
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Error</title>
	
    <!-- Google font -->
    <link href="https://fonts.googleapis.com/css?family=Montserrat:200,400,700" rel="stylesheet">
    <link rel="stylesheet" href="/css/notFound.css" type="text/css">
    <!--href="/css/notFound.css" 맨 앞의/는 static 까지의 경로를 알고 있다.-->
    <title>error</title>
</head>
<body>
    <div id="notfound">
        <div class="notfound">
            <div class="notfound-404">
                <h1>Sorry</h1>
                <h2>Error - 작업을 다시 확인해 주세요.</h2>
            </div>
            <!--history.go() : 이전 또는 이후페이지로 가도록 하는 함 -->
            <a href="javascript:history.go(-1)" style="color:#ff8b77; background:#fff">Go TO Back</a>
            <a href="/board/list">Go TO Board</a>
        </div>
    </div>
</body>
</html>
```

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Error</title>

    <!-- Google font -->
    <link href="https://fonts.googleapis.com/css?family=Montserrat:200,400,700" rel="stylesheet">
    <link rel="stylesheet" href="/css/notFound.css" type="text/css">
    <title>error</title>
</head>
<body>
<div id="notfound">
    <div class="notfound">
        <div class="notfound-404">
            <h1>Sorry</h1>
            <h2>Error 500 - 문제가 발생하였습니다.</h2>
        </div>
        <a href="javascript:history.go(-1)" style="color:#ff8b77; background:#fff">Go TO Back</a>
        <a href="/board/list">Go TO Board</a>
    </div>
</div>
</body>
</html>
```

## board 패키지에 list.html,read.html,update.html,write.html파일 생성하기
- list.html
```html
<!doctype html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="/css/main.css" type="text/css"/>
    <title>게시글 목록</title>
    <style>
        @media (max-width: 918px){
            .writer {display: none;}
            .regDate	 {display: none;}
            .updateDate {display: none;}
            div#searchWrap { display: block; text-align: center; }
            select{width: 100%;}
            a.search{ width: 100%;}
        }
        div#searchWrap { display: flex; text-align: center; }
        form[name='searchForm'] {overflow: hidden;}
        select{width: 30%;}
    </style>
</head>
<body class="is-preload">
<!-- Main -->
<div id="main">
    <div class="wrapper">
        <div class="inner">
            <!-- Elements -->
            <header class="major">
                <h1>Board</h1>
                <p>게시판 목록</p>
            </header>
            <!-- Table -->
            <h3><a class="write button small">글 등록</a></h3>
            <div class="table-wrapper">
                <table>
                    <thead>
                    <tr class="tHead">
                        <th class="bno">번호</th>
                        <th class="title">제목</th>
                        <th class="writer">작성자</th>
                        <th class="regDate">작성일</th>
                        <th class="updateDate">수정일</th>
                    </tr>
                    </thead>
                    <tbody>
                    </tbody>
                </table>
                <form name="searchForm">
                    <div class="fields">
                        <div class="field">
                            <div id="searchWrap">
                                <select name="type">
                                    <option value="">검색 기준</option>
                                    <option value="tcw">전체</option>
                                    <option value="t">제목</option>
                                    <option value="c">내용</option>
                                    <option value="w">작성자</option>
                                    <option value="tc">제목 또는 내용</option>
                                    <option value="tw">제목 또는 작성자</option>
                                    <option value="cw">내용 또는 작성자</option>
                                </select>
                                <input type="text">
                                <a href="" class="search button primary icon solid fa-search">검색</a>
                            </div>
                        </div>
                    </div>
                </form>
                <!--페이징 처리-->
            </div>
        </div>
    </div>
</div>
</body>
<!-- Scripts -->
<script src="/js/jquery.min.js"></script>
<script src="/js/jquery.dropotron.min.js"></script>
<script src="/js/browser.min.js"></script>
<script src="/js/breakpoints.min.js"></script>
<script src="/js/util.js"></script>
<script src="/js/main.js"></script>
</html>
```
- read.html
```html
<!doctype html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>게시글 상세보기</title>
    <link rel="stylesheet" href="/css/main.css" />
    <style>
        .uploadResult ul {
            display: flex;
            list-style: none;
        }
        h4 {
            margin: 0;
        }
        .line{
            border-bottom: 1px solid #ff8b77;
        }
        p {
            margin: 0;
        }

        textarea {
            resize: none;
        }
    </style>
</head>
<body class="is-preload">
<!-- Main -->
<div id="main">
    <div class="wrapper">
        <div class="inner">

            <!-- Elements -->
            <header class="major">
                <h1>Board</h1>
                <p>게시글 상세보기</p>
            </header>
            <!-- Table -->
            <h3><a class="list button small">목록 보기</a></h3>
            <div class="content">
                <div class="form">
                    <form method="post" th:action="">
                        <div class="fields">
                            <div class="field">
                                <h4>번호</h4>
                                <input type="text" readonly/>
                            </div>
                            <div class="field">
                                <h4>제목</h4>
                                <input type="text" readonly/>
                            </div>
                            <div class="field">
                                <h4>내용</h4>
                                <textarea rows="6" style="resize:none" readonly></textarea>
                            </div>
                            <div class="field">
                                <h4>작성자</h4>
                                <input type="text" readonly/>
                            </div>
                            <div class="field">
                                <h4>첨부파일</h4>
                                <div class="uploadResult">
                                    <ul>
                                    </ul>
                                </div>
                            </div>
                        </div>
                        <ul class="actions special">
                            <li>
                                <input type="button" class="button" value="수정" onclick=""/>
                                <input type="submit" class="button" value="삭제"/>
                            </li>
                        </ul>
                        <ul class="icons">
                            <li style="display: block">
                                <span class="icon solid fa-envelope"></span>
                                <strong>댓글</strong>
                            </li>
                        </ul>
                        <a href="javascript:void(0)" class="register button primary small" style="width: 100%">댓글 등록</a>
                        <div style="display: none" class="register-form">
                            <div>
                                <h4>작성자</h4>
                                <input type="text" name="replyWriter" placeholder="Replier">
                            </div>
                            <div>
                                <h4>댓글</h4>
                                <textarea rows="6" name="replyContent" placeholder="Reply" style="resize: none"></textarea>
                            </div>
                            <div style="text-align: right">
                                <a href="javascript:void(0)" class="finish button primary small">등록</a>
                                <a href="javascript:void(0)" class="cancel button primary small">취소</a>
                            </div>
                        </div>
                        <ul class="replies"></ul>
                    </form>
                    <div class="paging" style="text-align: center"></div>
                </div>
            </div>
        </div>
    </div>
</div>
</body>
<script src="/js/jquery.min.js"></script>
<script src="/js/jquery.dropotron.min.js"></script>
<script src="/js/browser.min.js"></script>
<script src="/js/breakpoints.min.js"></script>
<script src="/js/util.js"></script>
<script src="/js/main.js"></script>
</html>
```
- update.html
```html
<!doctype html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>게시글 수정</title>
    <link rel="stylesheet" href="/css/main.css" />
    <style>
        .uploadResult ul {
            display: flex;
            list-style: none;
        }

        .uploadResult ul li {
            position: relative;
        }

        .uploadResult ul li span {
            display: block;
            position: absolute;
            right: 10px;
            top: -25px;
            cursor: pointer;
            font-weight: bold;
            font-size: 1.5rem;
        }
    </style>
</head>
<body class="is-preload">
<!-- Main -->
<div id="main">
    <div class="wrapper">
        <div class="inner">

            <!-- Elements -->
            <header class="major">
                <h1>Board</h1>
                <p>게시글 수정</p>
            </header>
            <!-- Table -->
            <h3><a class="list button small">목록 보기</a></h3>
            <div class="content">
                <div class="form">
                    <form method="post" th:action="" id="updateForm" enctype="multipart/form-data">
                        <div class="fields">
                            <div class="field">
                                <h4>번호</h4>
                                <input type="text" readonly/>
                            </div>
                            <div class="field">
                                <h4>*제목</h4>
                                <input type="text"/>
                            </div>
                            <div class="field">
                                <h4>*내용</h4>
                                <textarea rows="6" style="resize:none"></textarea>
                            </div>
                            <div class="field">
                                <h4>작성자</h4>
                                <input type="text" readonly/>
                            </div>
                            <div class="field">
                                <h4>첨부파일</h4>
                                <input type="file" name="upload" multiple>
                            </div>
                            <div class="field">
                                <div class="uploadResult">
                                    <ul></ul>
                                </div>
                            </div>
                        </div>
                        <ul class="actions special">
                            <li>
                                <input type="submit" class="button" value="수정 완료"/>
                            </li>
                        </ul>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
</body>
<script src="/js/jquery.min.js"></script>
<script src="/js/jquery.dropotron.min.js"></script>
<script src="/js/browser.min.js"></script>
<script src="/js/breakpoints.min.js"></script>
<script src="/js/util.js"></script>
<script src="/js/main.js"></script>
</html>
```
- write.html
```html
<!doctype html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>게시글 작성</title>
    <link rel="stylesheet" href="/css/main.css" />
    <style>
        .uploadResult ul {
            display: flex;
            list-style: none;
        }

        .uploadResult ul li {
            position: relative;
        }

        .uploadResult ul li span {
            display: block;
            position: absolute;
            right: 10px;
            top: -25px;
            cursor: pointer;
            font-weight: bold;
            font-size: 1.5rem;
        }
    </style>
</head>
<body class="is-preload">
<!-- Main -->
<div id="main">
    <div class="wrapper">
        <div class="inner">
            <!-- Elements -->
            <header class="major">
                <h1>Board</h1>
                <p>게시글 등록</p>
            </header>
            <!-- Table -->
            <h3><a class="list button small">목록 보기</a></h3>
            <div class="content">
                <div class="form">
                    <form method="post" id="writeForm" enctype="multipart/form-data">
                        <div class="fields">
                            <div class="field">
                                <h4>제목</h4>
                                <input placeholder="Title" type="text" />
                            </div>
                            <div class="field">
                                <h4>내용</h4>
                                <textarea rows="6" placeholder="Content" style="resize:none"></textarea>
                            </div>
                            <div class="field">
                                <h4>작성자</h4>
                                <input placeholder="Writer" type="text" />
                            </div>
                            <div class="field">
                                <h4>첨부파일</h4>
                                <input type="file" name="upload" multiple>
                            </div>
                            <div class="field">
                                <div class="uploadResult">
                                    <ul></ul>
                                </div>
                            </div>
                        </div>
                        <ul class="actions special">
                            <li><input type="submit" class="button" value="등록" /></li>
                        </ul>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
</body>
<script src="/js/jquery.min.js"></script>
<script src="/js/jquery.dropotron.min.js"></script>
<script src="/js/browser.min.js"></script>
<script src="/js/breakpoints.min.js"></script>
<script src="/js/util.js"></script>
<script src="/js/main.js"></script>
</html>
```
