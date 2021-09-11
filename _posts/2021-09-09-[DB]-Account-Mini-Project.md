---
title: "[Database/JDBC] Account Mini Project"
excerpt : 

categories: 
  - Blog
tags:
  - [Database, java, Git]

toc: true
toc_sticky: true

date: 2021-09-09
last_modified_at: 2021-09-09

---

# Account Mini Project

# - 계좌 관리 프로그램

> ## 0. 사용 목적

​	- 우리가 이용하는 은행 서비스와 유사하게 통장(계좌)의 개설 및 계좌를 사용해서 서비스를 이용할 수 있는 프로그램을 만들어본다. 

> ## 1. 요구사항
>

1.  사용자는 계좌(통장) 개설이 가능하다.

2.  계좌 개설시 
   2.1 계좌번호, 계좌주명, 비밀번호, 잔액정보가 저장되어야 한다.

   2.2 최초 계좌 개설시 초기 납입액이 1000원 이상이 되어야 한다.

   2.3 계좌번호는 유일해야 하고 시스템에서 자동 발급한다.

3.  잔액조회시에 계좌번호가 존재해야 하고 계좌번호에 해당하는 비밀번호가 일치해야 한다.

4. 계좌에서 입금, 출금, 계좌이체가 가능하며, 입금액, 출금액, 계좌이체액은 모두 0원을 초과해야 한다.

   4.1 입금시 계좌번호, 비밀번호 일치

   4.2 출금시 계좌번호, 비밀번호 일치 및 잔액확인

   4.3 계좌이체시 입금자 및 예금주의 계좌가 각각 존재

   ​	4.3.1 송금자의 잔액확인, 비밀번호 일치여부 확인 

   ​	4.3.2 송금자에게서 출금 및 예금주에게 입금 처리가 정상적으로 수행될 경우에만 계좌이체 처리한다.

5. 계좌 잔액이 가장 높은 계좌주명과 잔액을 조회한다.

> ## 2. 요구사항 분석(주요 핵심기능 추출 )

- 위 요구사항에서 필요한 핵심기능들을 추출하면 코드에서 메서드를 구현할 때 더욱 편리해진다.

  1. 계좌 개설 - createAccount()
  2. 잔액조회 - findBalanceByAccountNo()
  3. 입금 - deposit()
  4. 출금 - withdraw()
  5. 계좌이체 - trasfer()
  6. 최고 잔액 계좌정보 0 fingHigestBalanceAccount() 

-  이렇게 6가지의 주요기능(메서드)으로 분류할 수 있으며, 요구사항의 조건들은 예외 생성 및 해당 메서드 내에서 예외처리를 통해 해결할 수 있다. 

-  핵심기능 메서드 외 getConnection(), checkAccount(), existsAccountNo() 메서드는 핵심 메서드를 구현하면서 반복되는 작업을 재사용하기 위하여 편리를 위해 구현한 메서드다. (+ DbInfo는 데이터와 연동을 위해 DB 정보를 담은 인터페이스)

- 프로젝트의 전박적인 구조를 UML(StarUML사용)을 통해 도식화하면 아래과 같다. 

  ![화면 캡처 2021-09-11 130249](https://user-images.githubusercontent.com/88620416/132935916-70fb85b6-a133-45d4-8e66-469107f98015.png)

## 3.  코드 

- 

```java
package model;

public interface DbInfo {
	String DRIVER = "oracle.jdbc.OracleDriver";
	String URL = "jdbc:oracle:thin:@127.0.0.1:1521:xe";
	String USERNAME = "scott";
	String PASSWORD = "tiger";
}
```



