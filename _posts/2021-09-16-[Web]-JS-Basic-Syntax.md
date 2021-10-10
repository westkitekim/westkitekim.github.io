---
title: "[Web/JS]JavaScript(1)-Basic Syntax"
excerpt:

categories:
  - Web
tags:
  [Web, java, Javascript]

toc: true
toc_sticky: true

date: 2021-09-21
last_modified_at: 2021-09-21
---

<style>
    #highlight {
        background-color: #e9e97f
    }
</style>


## JavaScript

- 웹 브라우저에서 작동하는 프로그래밍 언어이자 ECMAScript 표준을 준수하는 스크립트 언어 

자바스크립트는 객체(Object) 기반의 스크립트 언어로 주로 <span id="highlight">**웹의 동작을 구현**</span>하는데 사용된다. HTML에서는 웹 문서의 내용을 구성하고 CSS에서는 웹 문서의 레이아웃이나 색상, 스타일 등 디자인을 담당했다면 JS에서는 웹 문서의 각 요소를 필요에 따라 동적으로 움직이게 할 수 있다. 

아래는 자바스크립트의 예시 중 하나이다. 

![javascript-example1](https://user-images.githubusercontent.com/88620416/134262596-bd1cb279-8b2b-4f1d-8079-2ecd7c91a9e0.png)

메뉴 화면에서 '문제'라는 탭에 마우스를 가져가면 아래의 그림과 같이 같은 위치에 여러가지 하위 메뉴가 나타나는 것을 볼 수 있다. 이러한 동작이 자바스크립트를 통해 만들어 질 수 있다. 



## JavaScript Basic Syntax

```html
<script> "이곳에 자바스크립트 코드를 작성한다. "</script>

<script type="text/javascript">  "코드 작성"  </script>

<script>
    var a = 1;
    var a = 2;
    var b = 4;
    alert(a + b);
    
    // 한 줄 주석
    /*
    	여러 줄을 
    	작성합니다. 
    */
</script>
```

- 자바스크립트 코드는 위와 같이 `<script>` 태그 안에서 작성한다. 
- 자바스크립트의 주석은 JAVA언어와 동일하게 사용된다 `// - 한줄 주석, /* 내용 */ - 여러 줄 주석`
- 자바스크립트 코드는 html 문서 내의 head 태그나 body 태그 안에 모두 사용 가능하지만 관례적으로 head 태그 안에 작성한다. 

```html
<script type="text/javascript">
	console.log("내용 작성");
	//가장 많이 사용하는 function
	alert("내용 작성");
</script>
```

- console.log("내용 작성") : Java에서 System.out.println 역할과 동일하다. 웹 페이지에서 바로 출력 되지 않고 f12 클릭 후 Chrome 개발자 모드의 Console 창에서 "내용 작성" 문자열이 출력된 것을 확인할 수 있으며 매개변수가 있다면 변수의 내용이 출력된다. 
- alert("내용 작성") : 가장 많이 사용되는 function, 웹 페이지의 팝업창으로 alert() 안의 내용이 출력된다.

cf) Java 에서는 함수를 "메서드/메소드"라고 표현하는 반면 JavaScript 에서는 보통 "function" 으로 사용한다.



#### 외부의 Script 파일과 연결하여 JavaScript 작성

-------

```html
<script src="외부 스크립트 파일 경로 작성"></script>
```

- 외부의 js 파일과 연결할 경우 script 태그가 시작하는 부분에 src 속성을 통해 경로를 지정해준다. 
- 외부 자바스크립트 파일(.js) 은 `<script>` 없이 자바스크립트 소스만 바로 작성하고 확장자는 .js파일로 저장한다. 
- 외부 파일로 따로 생성하면 업데이트가 필요할 떄  js파일만 수정하면 되기 때문에 유지보수성이 향상되게 된다. 



### Variable(변수)

----

```html
<script type="text/javascript">
	var a=1; //중복선언 가능
	var a=2;
	alert(a);
	
	let b=1;
//	let b=2; // let은 중복선언이 불가, 같은 영역에서 동일한 변수명으로 let을 선언 
	b=2;// 재할당 가능
	alert(b);
	
	const c=1;
//	c=2;// 상수이므로 중복선언 및 재할당 불가
	alert(c);
</script>
```

- JavaScript 에서 변수 선언 및 정의는 데이터 타입을 명시하지 않는다. 

- ECMAScript : 다양한 브라우저에서 일관성 있는 동작을 위한 스크립트 표준을 말한다

  - ES5(ECMA5, 2009) : 변수 선언 var -> 중복선언 가능 ( Function-level scope )

  - ES6(ECMA6, 2015) : 변수 선언 추가 let -> 중복선언 불가 (Block-level scope ), 상수 선언 const 등 추가 

    자바의 지역변수와 유사하다 - 선언된 실행 영역에서만 사용가능

### Function

------

```html
<body>
	<script type="text/javascript"> 
		//함수 정의
		function f1() {
			console.log("f1 함수실행");
			alert("f1 함수실행");
		}
		function f2(message){
			alert(message);
		}
		function f3(info, num){
			//window 객체의 function alert() 함수
			//window 는 생략가능
			window.alert(info + " " + num);
		}
	</script>
	javascript function(함수) 테스트
	<!-- onclick 시 함수 호출 -->
	<input type="button" value="함수테스트1" onclick="f1()">
	<!-- 매개변수 함수, 문자열은 '," 다 가능  -->
	<input type="button" value="함수테스트2" onclick="f2('Hello')">
	<input type="button" value="함수테스트3" onclick="f3('Hello', 7)">
```

- onclick() 을 수행하면, 즉 웹 페이지 상에 생성된(input 태그) 각각의  버튼을 클릭하면 onclick 속성에 의해 함수가 호출되므로,  해당하는  f1(), f2(), f3() 함수가 실행되어 메세지가 출력된다. 
- window 객체는 생략 가능하다. 

### Confirm

------

```html
<script type="text/javascript">
	function test(){
		// confirm()은 반환값이 true/false인 확인창
		// 확인 누르면 true, 취소 누르면 false를 반환
		// window 객체명은 생략가능 
		let result=window.confirm("당신은 민초파입니까?");
		//alert(result);
		if(result) 
			alert("환영합니다. Welcome to Mincho world!");
		else
			alert("어서 나가주세요(단호)");
	}
</script>
```

- confirm() : true / false 로 반환되는 함수 
  alert 함수와 같이 팝업창으로 웹 페이지상에 출력되고 '확인' 과 '취소' 버튼이 있다. (true / false 선택)

-  confirm 함수도 window 객체의 메서드다.

- test() 함수가 호출되어 "당신은 민초파입니까?"라는 문구의 확인/취소 버튼이 있는 팝업창이 출력되고 각각의 버튼에 따라 위와 같은 메세지가 출력된다. 

  ​				 

```html
<script type="text/javascript">
	function enterMovie(){
		var result=confirm("만 19세 이상입니까?");//윈도우 객체명 생략가능
		if(result) {
			//alert("성인영화 즐감하세요!");
			//해당 url로 페이지가 이동된다
			location.href="http://pororopark.com";
			
		}else {
			alert("다음 기회에");
		}
	}
</script>
```

- confirm() 함수에서 확인 버튼을 클릭 시 (= 반환값이 true 일때) 특정 경로로 이동하고 싶다면 위와 같이 조건문을 통해 location 객체의 href 를 통해 경로를 지정해준다. 





### Reference

-------

- http://tcpschool.com/javascript/js_intro_basic

