---
title: "[자바의정석] Ch06.3.02 클래스변수와 인스턴스변수"

categories:
  - Java_NKS
tags:
  [Web, java, java_NKS]

toc: true
toc_sticky: true

date: 2023-06-04
last_modified_at: 2023-06-04
---

# [Chapter 06] 3.02 클래스변수와 인스턴스변수

## 1. 인스턴스변수(instance variable)

- 클래스 영역에 선언되며, 클래스의 인스턴스(객체)를 생성할 때 만들어진다.
- Therefore, 인스턴스 변수의 값을 읽거나 저장하기 위해서는 반드시 **객체를 생성**해야함
- 인스턴스마다 독립적인 저장공간을 가지므로 **인스턴스변수는 서로 다른 값을 가질 수 있다.**
- 인스턴스마다 고유한 상태값을 가져야하는 속성의 경우, 인스턴스 변수로 선언한다.

## 2. 클래스변수(class variable)

- 선언방법 : 인스턴스변수 앞에 static 붙임
- 클래스변수는 **모든 인스턴스가 공통된 저장공간(변수)을 공유**
- 어떤 클래스의 모든 인스턴스들이 고정된 공통값을 유지해야하는 속성의 경우, 클래스변수로 선언한다.
- metadata space에 저장되기 때문에 객체(instance)를 생성하지 않고도 바로 사용가능하다.
- '클래스이름.클래스변수' 와 같은 형식으로 사용.
- 클래스가 메모리에 '로딩(loading)'될 때 생성되어 프로그램이 종료될 때까지 유지되며, public 접근제어자 사용시 동일 프로그램 내에서 어디서나 접근 가능한 '전역변수(global variable)'의 성격을 갖는다.

## 3. 클래스변수와 인스턴스변수

![variable_card](https://e7.pngegg.com/pngimages/585/737/png-clipart-playing-card-card-game-hearts-spades-deck-of-cards-game-heart.png)

듀듕!!(이미지 작은거 찾기 귀찮아서 그냥 올림ㅎ)

인스턴스변수와 클래스변수의 차이의 예시로 카드클래스를 정의해본다.

카드클래스를 만든다고 가정했을 때, <br/>속성으로는 카드의 무늬, 숫자, 폭, 높이가 있다.

이중에서 카드의 무늬나 숫자가 달라져도 폭, 높이는 변하지 않고 모든 인스턴스(카드)가 공통적으로 같은 값으로 가져야하므로 클래스 변수로 선언한다.  <br/>그리고 카드 인스턴스는 자신만의 무늬(스페이스, 다이아몬스 .. )와 숫자(4, 6, A.. )를 가져야 하기 때문에 인스턴스 변수로 선언한다.

``` java
package baekjoon.graph;

class Card {
    // 인스턴스변수
    String kind;
    int number;
    // 클래스변수
    static int width = 100; // 카드의 폭
    static int height = 300; // 카드의 높이
}

public class CardTest {
    public static void main(String[] args) {
        System.out.println("Card.with = " + Card.width);
        System.out.println("Card.height = " + Card.height);

        Card c1 = new Card();
        c1.kind = "Diamond";
        c1.number = 4;

        // 2번째 카드로 인스턴스변수값 변경(A는 7로 사진과 별개로 설정)
        Card c2 = new Card();
        c2.kind = "Space";
        c2.number = 7;

        System.out.println("c1은 " + c1.kind + ", " + c1.number + "이며 크기는 (" + c1.width + ", " + c1.height + ") 이다.");
        System.out.println("c2은 " + c2.kind + ", " + c2.number + "이며 크기는 (" + c2.width + ", " + c2.height + ") 이다.");
        System.out.println("======================");
        System.out.println("c1의 width와 height를 각각 50, 80으로 변경한다.");
        System.out.println("======================");
        // 클래스변수 변경
        c1.width = 50;
        c1.height = 80;

        System.out.println("c1은 " + c1.kind + ", " + c1.number + "이며 크기는 (" + c1.width + ", " + c1.height + ") 이다.");
        System.out.println("c2은 " + c2.kind + ", " + c2.number + "이며 크기는 (" + c2.width + ", " + c2.height + ") 이다.");

    }
}

```

> **인스턴스변수는 인스턴스가 생성될 때마다 생성되므로 인스턴스마다 각기 다른 값을 유지.<br/>하지만 클래스변수는 모든 인스턴스가 하나의 저장공간을 공유하므로, 항상 공통된 값을 갖는다.**

