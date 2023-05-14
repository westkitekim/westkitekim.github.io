---
title: "[자바의정석] Ch09.2.07 BigDecimal"

categories:
  - Java_NKS
tags:
  [Web, java, java_NKS]

toc: true
toc_sticky: true

date: 2023-05-14
last_modified_at: 2023-05-14
---

# [Chapter 09] 2.07 BigDecimal

## 1. java.math.BigDeimal 클래스

- **BigDecimal**은 **Java** 언어에서 숫자를 정밀하게 저장하고 표현할 수 있는 유일한 방법이다.
- **Java** 언어에서 돈과 소수점을 다룬다면 **BigDecimal**은 선택이 아니라 필수이다.
- 소수점을 저장할 수 있는 가장 크기가 큰 타입인 **double**은 소수점의 정밀도에 있어 한계가 있어 값이 유실될 수 있다.
- **BigDecimal**의 유일한 단점은 느린 속도와 기본 타입보다 조금 불편한 사용법 뿐이다.

double타입으로 표현 할 수 있는 값은 최대 13자리까지만 표현이 가능하고,  실수형의 특성상 오차를 피할 수 없다.<br/>BigDecimal은 정수를 이용해서 실수를 표현한다. <br/>실수의 오차는 10진수를 2진수로 정확히 변환할 수 없는 경우가 있다.  때문에 오차가 없는 2진 정수로 변환하여 다룬다. <br/>**<center>정수 X 10<sup>-scale</sup></center>**

scale은 0부터 Integer.MAX_VALUE 사이의 범위에 있는 값이다.<br/>그리고 BigDecimal은 정수를 저장하는데 BigInteger를 사용한다.

```java
 private final BigInteger intVal; 	    // 정수(unscaled value)
 private final int scale;            	// 지수(scale)
 private transient int precision;    	// 정밀도(precision) - 정수의 자릿수
```

## 2. BigDecimal의 생성

BigDecimal을 생성하는데 여러 가지 방법이 있지만, **일반적으로 문자열로 숫자를 표현하여 생성**한다.

>  *주의!* **double타입의 값으로 BigDecimal 생성시 오차가 발생할 수 있다.**

```java
BigDecimal val;
val = new BigDecimal("123.4567890"); 		// 문자열로 생성
val = new BigDecimal(123.456);			    // double타입의 리터럴로 생성
val = new BigDecimal(123456);			    // int, long타입의 리터럴로 생성가능
val = BigDecimal.valueOf(123.456);			// 생성자 대신 valueOf(double) 사용
val = BigDecimal.valueOf(123456); 			// 생성자 대신 valueOf(int) 사용

System.out.println(new BigDecimal(0.1)); 	 // 0.10000000000000000555111...
System.out.println(new BigDecimal("0.1"));	 // 0.1
```

## 3. BigDecimal의 연산

```java
BigDecimal add(BigDecimal val)	        // 덧셈 (this + val)
BigDecimal substract(BigDecimal val)	// 뺄셈 (this - val)
BigDecimal multiply(BigDecimal val)		// 곱셈 (this * val)
BigDecimal divide(BigDecimal val)	    // 나눗셈 (this / val)
BigDecimal remainder(BigDecimal val)	// 나머지 (this % val)
```

> 주의!! 
>
> - BigInteger, BigDecimal 모두 불변(immutable)이므로, 반환타입이 BigDecimal인 경우 새로운 인스턴스가 반환된다.<br/>***연산 후 새로운 인스턴스에 담아줘야 연산된 결과가 반환된다.***
>
>   ```java
>   BigDecimal val = new BigDecimal("100");
>   BigDecimal addVal = new BigDecimal("15");
>   
>   val.add(addVal);
>   System.out.println(val); 	// 100
>   
>   BigDecimal result = val.add(addVal);
>   System.out.println(result);				// 115 
>   System.out.println(val.add(addVal));	 // 115 
>   ```
>
>   



#### Reference

- [https://jsonobject.tistory.com/466](https://jsonobject.tistory.com/466)

  
