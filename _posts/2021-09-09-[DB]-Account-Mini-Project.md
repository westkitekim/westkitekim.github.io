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
   
3. 잔액조회시에 계좌번호가 존재해야 하고 계좌번호에 해당하는 비밀번호가 일치해야 한다.

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

- 이렇게 6가지의 주요기능(메서드)으로 분류할 수 있으며, 요구사항의 조건들은 예외 생성 및 해당 메서드 내에서 예외처리를 통해 해결할 수 있다. 

- 핵심기능 메서드 외에 getConnection(), checkAccount(), existsAccountNo() 메서드는 핵심 메서드를 구현하면서 반복되는 작업을 재사용하기 위하여 편리를 위해 구현한 메서드다. (+ DbInfo는 데이터와 연동을 위해 DB 정보를 담은 인터페이스)

- 요구사항을 확인해보면 총 5개의 조건을 확인할 수 있다. 이 조건들은 5개의 예외처리를 통해 해결할 수 있다.(모두 사용자 정의 예외)

  1.  CreateAccountException : 최초 계좌 개설시 초기 납입액 1000원 이상
  2.  AccountNotFoundException : 계좌의 존재 여부 확인
  3.  NotMatchedPasswordException : 계좌번호의 비밀번호 일치여부 확인
  4.  NoMoneyException  : 입금액, 출금액, 이체액 0원 초과 
  5. InsufficientBalanceException : 계좌이체시 송금자의 잔액이 이체액보다 적을 경우 이체 불가

- 프로젝트의 전박적인 구조를 UML(StarUML사용)을 통해 도식화하면 아래과 같다. 

  ![화면 캡처 2021-09-11 130249](https://user-images.githubusercontent.com/88620416/132935916-70fb85b6-a133-45d4-8e66-469107f98015.png)

> ## 3.  구현

### 3 - 0. Database 

### 3 - 1. DbInfo Interface 

- OracleDriver class 경로, Oracle url, username, password 와 같이 자주 재사용될 데이터들은 인터페이스에서 상수로 정의한다. 

### 3 - 2. AccountVO class : 계좌의 정보가 담긴 클래스 ( VO : Value Object)

- 객체(계좌)의 정보를 담는 AccountVO 클래스에는 정보가 담길 instance variable, Constructor, getter/setter method, toString method가 필요하다.

  -- 인스턴스 변수에는 accountVO: String, name: String, password: String, balance: int 총 4개가 있다.

- 필요에 의해 기본생성자를 비롯하여 다양한 매개변수를 가진 생성자가 필요하다.  

  -- 기본생성자는 setter/getter 메서드 이용시 사용.

  -- primary key 인 accountVo를 제외한 3개의 매개변수를 가진 생성자는 insert/ update/ delete시에 사용

  -- 모든 iv가 들어있는 생성자는 select 조회시에 사용되는 등 다양하다. 

- 다음 코드에서는 편의를 위해 getter/setter 메서드와 toString 메서드는 생략했다. sql 구문에 따라 필요한 생성자가 다른 것에 주의하자 

  

### 3 -3. AccountDAO class : DB에서 데이터를 조회, 조작하는 기능을 가진 클래스(DAO : Date Access Object)

- 상단의 요구사항 분석에 서술된 핵심 기능 위주로 메서드를 구현해본다 

- jdbc를 통해 DB에 연결하는 코드들은 중복이 되므로 편의상 생략했다.(Connection, PreparedStatement, ResultSet, closeAll()... etc)

  #### 3 - 3. 1 계좌 개설 메서드 (createAccount())

  - 예외처리 : CreateAccountException
  - main 메서드를 통해 AccountVO객체가 전달이 되면 insert sql 구문을 통해 DB에 데이터를 넣어준다 
  - 초기 납입액 1000원 이상의 조건은 메서드 상단부에서 먼저 처리하고 진행한다 

  ```java
  public void createAccount(AccountVO accountVO) throws SQLException, CreateAccountException {
  		//if 조건문을 먼저 상단에 해줘야 조건에 맞지 않을 시 아래 구문을 수행하지 않고 
  		//바로 예외처리를 하여 벗어난다 (예외 던지고 main에서 catch로 처리하고 끝)
  		if(accountVO.getBalance() < 1000) 
  			throw new CreateAccountException("계좌 개설시 초기 납입금은 1000원 이상이어야 합니다");
  		Connection con = null;
  		PreparedStatement pstmt = null;
  		try {
  			con = getConnection();
  			String sql = "insert into account(account_no, name, password, balance) values(account_seq.nextval, ?, ?, ?)";
  			pstmt = con.prepareStatement(sql);
  			pstmt.setString(1, accountVO.getName());
  			//.... 값에 할당하는 과정 생략 
  			pstmt.executeUpdate();
  		}finally {
  			closeAll(pstmt, getConnection());
  		}
  	}
  ```

  

  #### 3 - 3. 2 잔액 조회 메서드 (findBalanceByAccountNo())

  - 예외처리 : NotMatchedPasswordException, AccountNotFoundException
  - 잔액 조회는 2가지 예외상황(계좌 존재유무, 비밀번호 일치여부)이기 때문에 1행을 먼저 뽑고 그 안에서 다시 예외를 확인한다는 점에 주의하자. 
  - sql문과 예외처리 if 조건문 확인
  - 뒤에 나올 checkAccountNoAndPassword()는 잔액조회 메서드와 모두 동일하나 return 값만 없다.

```java

	public int findBalanceByAccountNo(String accountNo, String password) throws SQLException, AccountNotFoundException, NotMatchedPasswordException {
        ...
			String sql = "select password, balance from account where account_no = ?";
			...
			//exception이 발생하면 return되지 않고 바로 catch로 날아간다 
			if(rs.next()) {//계좌번호에 해당하는 계좌가 있는 경우
				if(rs.getString(1).equals(password)) {//비밀번호가 일치하면
					//2번째 값인 balance 반환 
					balance = rs.getInt(2);
				}else {
					throw new NotMatchedPasswordException("계좌의 패스워드가 일치하지 않습니다");
				}	
			}else {//계좌번호에 해당하는 계좌가 존재하지 않는 경우
				   //사용자 정의 예외 발생시킨다
				throw new AccountNotFoundException(accountNo + "계좌정보에 해당하는 계좌가 존재하지 않습니다");
	...
		return balance;
	}
```

#### 	3 - 3. 4 계좌 입금 메서드 (deposit()) 및 출금 메서드(withdraw())

- 예외 처리 : checkAccountNoAndPassword을 통해 2가지 예외(계좌번호 확인, 패스워드 일치)를 처리한다.
- 원래 구현했던 코드는 balance라는 변수에 findBalanceByAccount()메서드의 잔액을 넣어주고 다음과 같이 구현했었다.

```java
String sql = "update account set balance = ? where account_no = ?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, balance + money);
```

- 하지만 이 코드는 값을 넣어줄 때 연산이 되기 때문에 여러 명의 사용자가 입금을 할 때 잔액의 변동사항이 생길 수 있기 때문에 완전히 무결성을 충족하는 코드라고 보기는 어렵다.

  따라서 아래와 같이 update sql 구문 안에서 연산을 처리하는 코드로 리팩토링한다.

```java
	public void deposit(String accountNo, String password, int money) throws NoMoneyException, SQLException, AccountNotFoundException, NotMatchedPasswordException {
		if(money <= 0) {
			throw new NoMoneyException("입금액은 0원을 초과해야 합니다");
		}
		checkAccountNoAndPassword(accountNo, password);
		...
		try { 
			con = getConnection();
			String sql = "update account set balance = balance + ? where account_no = ?";
			...
		}finally {
			closeAll(pstmt, con);
		}
	}
```

































