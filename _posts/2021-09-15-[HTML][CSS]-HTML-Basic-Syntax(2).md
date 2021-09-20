---
title: "[HTML] HTML Basic Syntax-(2)"
excerpt:

categories:
  - Web
tags:
  [HTML, CSS, Javascript]

toc: true
toc_sticky: true

date: 2021-09-20
last_modified_at: 2021-09-20
---



## CSS Basic Selector(html 에서 css  적용)

```html
<style> "여기 안에 디자인을 적용할 CSS 코드 구현" </style>
```

CSS 디자인은 HTML 문서에서 `<style>` 태그 안에 구현된다.

```html
<link rel="stylesheet" type="text/css" href="css/mystyle.css">
```

하지만 만약 CSS 파일이 외부에 작성되어 있을 경우 위와 같이 HTML 문서의 `<head>` 태그 안에서 경로를 알려줘야 한다. 이 때 `<link>` 태그를 이용하여 작성하는데, link 태그 안에서 href 속성의 경로로 CSS 파일을 가져온다. 

### Universial Selector(전체 선택자)

```html
* { 속성: 값; 속성: 값; .....}  <!-- 기본 syntax -->
```

```html
* { background-color: lime; }
```

전체 선택자는 말 그대로 전체를 선택한다는 뜻이다. 문서의 모든 요소에 스타일(디자인)을 적용할 때 사용한다.  가장 앞에 전체를 나타내는 `*` 기호를 사용한다.

예시의 경우 body 부분에 텍스트 요소들만 있다면 글의 배경색은 모두 라임색이 된다. 



### Type Selector(타입 선택자)

```html
태그명 { 스타일 규칙 }  <!-- 기본 syntax -->
```

```html
..
<style>
	p {
		font-style: italic;
	}
</style>
..
<body>
    <p> 예시입니다. </p>
    <p> p태그의 모든 텍스트에 </p>
    <p> 기울임 글꼴 스타일이 적용됩니다. </p>
</body>
```

타입 선택자는 특정 태그를 사용하는 모든 요소에 스타일을 적용한다. 따라서 블럭 바깥에 태그명을 명시하고 스타일을 적용한다. 

### Class Selector(클래스 선택자)  / Id Selector(id 선택자)

```html
.클래스명 { 스타일 규칙 } <!-- 기본 syntax -->
```

```html
...
<title>CSS study</title>
<style type="text/css">
	li{
		color: orange;
	
	}
	/*
		css 선택자 : 클래스를 선택할 때는 .클래스명 , 아이디를 선택할 때는 #아이디명 으로 사용 
		디자인쪽에서는 class 를 많이 사용하고 개발 부분에서는 1개만 선택해야 해서 id 많이 사용
		
		class 나 id 지정해준 애들을 우선순위가 더 높다?
		기본적으로 순차적인 구조지만 지정해준 애들이 우선순위 더 높음
	*/
	.ct{ 
        color: lime; 
    }
</style>
</head>
<body>
	<ul>
		<li>css는 웹사이트의 디자인 스타일을 담당</li>
		<li>html은 웹문서의 컨텐트를 담당</li>
		<li class="ct">javascript는 웹문서의 행위를 담당</li>
		<li>jsp는 서버의 동적인 페이지 구현을 담당 </li>
		<li class="ct">Servlet은 java web application의 기반기술로서 컨트롤러의 역할을 담당</li>
	</ul>
</body>
</html>

```

**클래스 선택자**는 특정 부분만 선택해서 스타일을 적용한다. 앞서 살펴보았던 타입 선택자는 해당하는 태그의 모든 요소에 스타일을 지정하지만 예시와 같이 list의 속성 `<li>` 으로 class 이름을 부여한다면 원하는 부분만 스타일을 적용할 수 있게 된다. 

기본형은 클래스명 앞에 . 을 적어 특정부분만 적용시킨다 .

위의 예시에서 배경색은 `<li>` 태그 안의 전체에 해당하는 텍스트의 색깔을 오렌지 색으로 적용했다.하지만 `<li>` 의 속성 중에서 이름이 ct인 클래스의 텍스트는 라임색으로 지정하여 해당 클래스의 색깔은 다르게 나오게 된다. 

기본적으로 CSS는 순차적인 구조로 적용이 되긴 하지만 (일반적으로 순서대로 코드가 작성되기 때문) 범위가 좁아질 수록 우선순위가 높아진다고 할 수 있다.(전체 선택자 - 타입선택자 - 클래스 선택자 / id 선택자)

```html
#아이디명 { 스타일 규칙 } <!-- 기본 syntax -->
```

**id 선택자**도 역시 클래스 선택자와 비슷하게 웹 문서의 특정  부분을 선택하여 스타일을 적용시키는데 사용된다. 대신 클래스와의 차이점이라면 클래스 선택자는 문서에서 여러 번 적용할 수 있는 반면, id 선택자는 문서에서  한 번만 적용할 수 있다는 것이다. 

클래스 선택자와 id 선택자는 속성값으로 사용된다. 





