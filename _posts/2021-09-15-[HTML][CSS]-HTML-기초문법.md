---
title: 
excerpt:

categories:
  - Database
tags:
  [HTML, CSS, Javascript]

toc: true
toc_sticky: true

date: 2021-09-15
last_modified_at: 2021-09-15
---

## HTML

--------------------------------------

HyperText Markup Language의 약자로 웹 문서의 내용을 정의하는 언어로 HTML에는 서로 연결한다는 의미가 포함되어 있다. 웹 브라우저에 의해 서버에서 전송된 html 정보를 입력받아 번역해서 웹 화면을 구성하게 한다. (서버는 html 텍스트를 출력하고 웹 브라우저는 이것을 번역하여 화면으로 구성)cf) HyperText는 문서를 서로 연결해주는 링크를  뜻하고,  Markup은 표시한다는 의미를 뜻한다.

**웹 브라우저에 내용을 보여주는 텍스트, 이미지, 영상 등의 위치를 표시하는 것.**
**웹 페이지를 만들기 위한 언어로 웹 브라우저에 위에서 동작하는 언어. **

## CSS

------

웹 문서의 디자인 스타일 및 레이아웃을 담당(웹 디자인 담당)
tag : `<style> style태그 내부에tj 디자인 구성 </style>`

## JavaScript

-----

웹 문서의 행위를 담당한다. (자바스크립트의 기본 역할) 웹 문서의 각 요소를 가져와서 필요에 따라 스타일을 변경하거나, 움직이게 하여 동적인 역할을 담당한다. 

## XML(eXtensible Markup Language)

------

주로 설정 정보를 명시할 때 사용하는 확장형(다목적) Markup 언어. HTML처럼 데이터를 보여주는 목적이 아니라 데이터를 저장하고 전달할 목적으로 만들어진 언어다. 
또한 HTML은 이미 만들어져 있는 태그만 사용이 가능하지만 XML 사용자가 임의로 태그를 생성할 수 있다. 



## HTML 기본구성

---------

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<!--<body>태그 사이에 웹페이지에 들어갈 내용을 작성해주세요 -->
</body>
</html>
```

이클립스에서 Dynamic Web Project 를 생성한 다음 src - main - webapp에서 html 파일을 만들면 기본적으로 제공되는 코드다. java에서는 src 폴더에 소스코드가 저장되었던 반면 Dynamic Web Project에서는 src - main - webapp 폴더에 저장됨을 기억하자. 

큰 구조를 먼저 파악해본다면 html은 **`<head>`** 와 **`<body>`** 로 나뉜다. 다른 태그들과 함께 밑에서 하나씩 확인해보자. 


```html
<태그명> 내용 적어주세요 </태그명>
	<태그명>은 태그의 시작
    </태그명>은 태그의 마지막
```

먼저 html 에서는 어떤 대상에 대한 표시로 위와 같은 태그라는 것을 사용한다. 
표시하고 싶은 대상의 앞 뒤에 태그를 사용하는데, 태그 이름을 `<` 와 `>` 사이에 적어주고 내용을 시작한다. 그리고 내용의 마지막에 태그를 닫아준다는 의미로 **`</`**와 **`>`** 사이에 태그 이름을 넣어주어 내용을 마무리한다.

- **`<!-- 내용 -->`** : html에서 주석이 필요할 때 `<!--` 과 `-->` 안에서 사용한다.
- **`<!DOCTYPE html>`** :HTML5의 선언부. 가장 많이 사용하는게 HTML5라서 뒤의 숫자는 빼고 사용한다. (cf. 기존 시스템의 부분적인 업데이트나 운영할 시 HTML도 있을 수 있지만 HTML5의 기능 지원 태그들이 달라질 수 있다.)

```html
1.
<head>
    ...
</head>
2. 
<body>
    ...
</body>
```

> **html의 구조는 크게 head와 body로 나뉜다.**

- **`<head> ..... </head>`** : head 태그 안에 웹 문서의 메타데이터(데이터의 데이터, 현재 문서의 설정 정보)를 기술한다.
- **`<body> ..... </body>`** : body 태그 안에 웹 페이지 화면에 나타날 컨텐트를 기술한다.

- **`<meta charset="UTF-8">`** :  html 파일의 인코딩 형식을 알려주는 태그, 다국어 지원이 가능한 UTF-8 형식을 사용한다.
- **`<title>Insert title here</title>`** :  html 파일의 제목을 나타내는 태그. 초기값인 Insert title here를 지우고 원하는 제목을 넣어준다.

## Tags

-----------

```html
<marquee> marquee 안에 있는 내용이 좌우상하로 움직인다 </marquee>
```

#### Attributes(속성)

- width : 스크롤 영역의 가로 너비 설정(ex. `<marquee width="200px"> 내용 </marquee>` )

- height : 스크롤 영역의 세로 높이 설정

- direction : 스크롤 방향 설정(left, right, up, down)

- loop : 스크롤 반복 횟수

- bgcolor : 스크롤 영역의 배경색상 설정

- scrollmount : 스크롤의 픽셀 수 설정

- scrolldelay : 스크롤의 이동속도 (단위 : 1/1000초, ex. `<marquee scrolldelay="100"> 내용 </marquee>`)

  ------

```html
<font> 해당 영역 내부의 폰트 크기 및 색상 등 설정 </font>
```

#### Attributes(속성)

- size : 폰트 크기

- color : 폰트 색상

  -----

```html
<br> : 줄바꿈
<br><br> : 2번 줄바꿈
... 원하는 만큼 줄 바꾸고 싶을 때 사용
```

```html
<!-- 웹 페이지에 이미지 삽입하기 -->
<img src="폴더경로명/이미지파일명.확장자명"> 
```

- 이미지를 삽입할 경우 src에 파일의 경로를 넣어준다. 이미지 파일은 보통 별도의 폴더에서 관리한다. 

- 상위 경로로 이동시 `../` 을 사용한다. 

  ---------

```html
<a> anchor 의 첫 문자, 다른 문서로 이동, 이 내용을 클릭하면 해당 경로로 이동합니다 </a>
```

#### Attributes(속성)

```html
<a href="../cat.html">cat page로 이동</a>
<a href="../cat.html" target="_self"></a>
```

- href : hypertext reference, 참조하는 링크의 경로를 작성한다. 절대경로, 상대경로

- target : 내용(링크)을 클릭시 창을 여는 방법을 지정한다.

  - _self : 링크를 클릭한 해당 창에서 열기
  - _blank : 링크를 새 창으로 열기
  - _parent : 부모 창에서 열기 (부모 창이 없다면 _self 로 처리)
  - _top : 전체 브라우저 창에서 가장 상위의 창에서 열기(부모 창이 없다면 _self 로 처리 )

  ------

```html
<!-- 목록 만들기 -->
<ol>
    order list : 순서가 없는 목록 생성하기
    <li> list, 목록의 내용을 작성한다 </li>
   	<li> list, 목록의 내용을 작성한다 </li>
    ....
</ol>

<ul>
    unordereded list : 순서가 있는 목록 생성하기(1, 2, 3 ...)
    <li> list, 목록의 내용을 작성한다 </li>
   	<li> list, 목록의 내용을 작성한다 </li>
    ....
</ul>

```

-----

```html
<table> 테이블(표) 만들기 </table>

<table>
    <tr> <!-- tr : table row, 한 행을 만드는 태그  -->
        <td> table data : 한 셀에 들어갈 내용을 넣는다 </td>
    </tr>
</table>

<!-- 2행 3열 만들기 --> 
<table>
    <tr>
        <td>번호</td><td>이름</td><td>나이</td>
    </tr>
    <tr>
        <td>1</td><td>홍길동</td><td>67</td>
    </tr>
    <tr>
        <td>2</td><td>김철수</td><td>99</td>
    </tr>
</table>
```

#### Attributes(속성)

```html
<table border="1">
   <tr>
 		<td colspan="2" align="center">신청서</td> 
 	</tr>
 	<tr>
 		<td>이름</td><td>아이유</td>
 	</tr>
 	<tr>
 		<td>나이</td><td>30세</td>
 	</tr>
</table>
```

- border : 표 테두리의 굵기 (경계 만들기)
- align : 셀 내에서 정렬(왼쪽 정렬, 중앙 정렬, 오른쪽 정렬)
- colspan : 주어진 수 만큼의 열의 개수 병합
- rowspan : 주어진 수 만큼의 행의 개수 병합 

![html_table_border_colspan](https://user-images.githubusercontent.com/88620416/133784854-00a95cb6-8a7d-4005-b4a4-028c2ce48e97.png)

위 코드를 실행하면 웹 브라우저에 다음과 같이 출력되는 것을 확인할 수 있다.



-----

#### Reference

- https://sharp57dev.tistory.com/39
- https://electronic-moongchi.tistory.com/87
- http://www.tcpschool.com/xml/xml_intro_basic