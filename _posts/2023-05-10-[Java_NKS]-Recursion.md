---
title: "[자바의정석] Ch06.3.10 재귀함수(재귀호출, Recursion)"

categories:
  - Java_NKS
tags:
  [Web, java, java_NKS]

toc: true
toc_sticky: true

date: 2023-05-10
last_modified_at: 2023-05-10
---

# [Chapter 06] 3.10 재귀호출(recursive call)

## 1. 재귀함수란?

메서드 내부에서 메서드 자신을 다시 호출하는 함수를 의미한다.

- 어떤 함수가 매개변수만 바꾸어 자기 스스로를 호출하는 방식
- 큰 문제를 반복 적용 가능한 작은 문제로 나눠 푸는 방법

```java
void recursion() {
    recursion(); // 재귀함수 호출
}
```

재귀 함수 호출과 다른 함수를 호출하는 것은 큰 차이가 없다. 
메서드 입장에서는 '메서드 호출'이라는 것이 특정 위치에 저장되어 있는 **일련의 명령어**들을 수행하는 것 뿐이기 때문이다.

재귀 함수는 반복문을 다르게 표현한 방식이라고 볼 수 있다. <br/>그래서 반복문과 유사한 점이 많으며(ex. 함수의 종료를 위한 조건문 필수), 대부분의 재귀호출은 반복문으로 작성하는 것이 가능하다.<br/>하지만 반복문은 그저 같은 명령어들만 반복해서 수행하지만, 재귀함수로 메서드를 반복적으로 호출하는 것은 몇 가지 과정이 추가로 필요하기 때문에 반복문보다 수행시간이 더 오래 걸린다.<br/>(메서드 호출 과정 : 매개변수 복사와 종료 후 복귀할 주소 저장 등이 있음)

호출된 메서드는 '값에 의한 호출(call by value)'을 통해, 원래의 값이 아닌 복사된 값으로 작업하기 때문에 호출한 메서드와 **관계없이** **독립적인 작업수행**이 가능하다.

## 2. 재귀함수 특징

1. **반드시 종료지점이 있어야 한다. == 조건문이 필수적으로 있어야 한다.** 
   - 즉, 함수 내부에 재귀호출뿐이면 무한히 자기 자신을 호출하기 때문에 무한 반복(무한 루프)에 빠지게 된다. 무한 반복문이 조건문과 함께 사용되어야 하는 것처럼, 재귀함수는 반드시 종료되어야하기 때문에 **조건문이 필수**적으로 따라다닌다.
2. 재귀가 필요한 알고리즘이 많다. (ex. 이진 검색, 트리 탐색, 퀵 정렬, 그래프 알고리즘 등)
3. 반복문에 비해 비효율적이나, **논리적 간결성**을 목적으로 사용한다.

## 3. 재귀함수 구현

대표적인 재귀함수 구현방법에는 팩토리얼(factorial)이 있다.

```java
class FactorialTest {
    public static void main(String args[]) {
        int result = factorial(4); // 4! = 4 * 3 * 2 * 1

        System.out.println(result);
    }

    static int factorial(int n) {
        int result = 0;

        if (n == 1) result = 1; // 재귀함수 종료 조건 1! = 1 ( => f(1) = 1 )
        else result = n * factorial(n - 1); // 재귀함수 호출 : 파라미터가 1이 아니면 1을 뺀 값을 다시 파라미터로 넣어서 재귀함수로 호출한다.

        return result;
    }
}
```



## 4. 재귀함수 구현시 주의사항

### 스택오버플로우(StackOverFlow)

상단의 코드에서 매개변수 n의 값이 0인 경우 if문의 조건식이 참이 될 수 없기 때문에 계속해서 재귀호출만 일어나고 메서드가 종료되지 않으므로 스택에 계속 데이터가 쌓이게 되면서 **스택오버플로우 에러(StackOverflow Error)**가 발생한다.

따라서 메서드 작성시, 아래 코드처럼 어떤 값이 들어와도 에러없이 처리될 수 있도록 **'매개변수 유효성검사'**를 해줘야 한다.

```java
class FactorialTest {
    public static void main(String args[]) {
        int n = 21;
        long result = 0;

        for (int i = 1; i <= n; i++) {
            result = factorial(i);
            
            // 매개변수 유효성 검사
            if (result == -1) {
                System.out.printf("유효하지 않은 값입니다. (0 < n <= 20) : %d%n", n); // println 사용X
                break;
            }

            System.out.printf("%2d! = %20d%n", i, result);
        }
    }

    static long factorial(int n) {
        if (n <= 0 || n > 20) return -1; // 매개변수 유효성 검사 추가
        if (n <= 1) return 1; // 재귀함수 종료 조건 1! = 1 ( => f(1) = 1 )
        return n * factorial(n - 1); // 재귀함수 호출 : 파라미터가 1이 아니면 1을 뺀 값을 다시 파라미터로 넣어서 재귀함수로 호출한다.
    }
}
```



#### Reference

- [https://data-marketing-bk.tistory.com/27](https://data-marketing-bk.tistory.com/27)
