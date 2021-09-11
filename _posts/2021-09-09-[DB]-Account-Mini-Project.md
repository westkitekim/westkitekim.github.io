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

#### 	3 - 3. 4 계좌 입금 메서드 (deposit()) 

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

#### 	

#### 	3 - 3. 5 계좌 출금 메서드 (withdraw())

- 출금메서드는 sql 구문에서 money를 빼주는 것과 입금 메서드에서 예외처리를 InsufficientBalanceException을 추가하는 것 외에 코드는 거의 동일하다 .

- 이체하려는 금액이 잔액보다 많을 경우에 발생하는 예외를 입금메서드의 예외에서 추가한다 (총 4가지 예외)

  ```java
  if(money <= 0) {
  			throw new NoMoneyException("출금액은 0원을 초과해야 합니다");
  		}
  		int balance = findBalanceByAccountNo(accountNo, password);
  		if(balance < money) {
  			throw new InsufficientBalanceException("잔액보다 출금액이 커서 출금할 수 없습니다");
  		}
  ```

####    3 - 3. 6 계좌이체 메서드(transfer())

- 주의할 점>
  1. 계좌이체에서는 입금자 계좌에서의 출금과 예금주 계좌에 입금 과정이 한 몸처럼 같이 진행돼야 하기 때문에 transaction 처리가 필요하다. 
  2. withdraw() 출금 메서드를 사용해도 sql 을 2번 작성한 결과와 동일하게 나올 수 있지만 transaction의 진행은 오직 1개의 Connection에서만 1개의 Session에서만 진행되어야 하기 때문에 좋은 코드라고 할 수 없다. 
     - withdraw를 사용되도 정상적으로 결과값이 나오는 이유? Connection 안의 withdraw() Connection이 있기 때문에 겉으로는 1개의 Connection이라고 인식하는 것으로 예상해본다..(추후 Update필요!! )
  3. 예외처리가 많아지면 가장 간단한 예외부터 처리한다.(자원낭비 최소화의 목적) -  NoMoneyException

-  transaction 처리를 위해 필요한 것은 무엇일까? 

  1. setAutoCommit(false) - 자동 커밋모드 해제
  2. commit() - 정상적으로 수행이 완료됐으면 db 연동
  3. rollback() - 하나라도 정상적으로 수행이 안됐으면 원래 상태로 복귀
  4. throw - 예외가 발생한 상황을 구현부 안에서만 처리하는 것이 아니라 사용자에게 알려주기 위해 예외 던진다.

  ```java
  	public void transfer(String senderAccountNo, String password, int money, String receiverAccountNo) throws NoMoneyException, SQLException, AccountNotFoundException, NotMatchedPasswordException, InsufficientBalanceException {
  		//0원 초과 예외확인
  		if(money <= 0) {
  			throw new NoMoneyException("이체액은 0원을 초과해야 합니다");
  		}
  		//수금자 계좌번호 확인
  		if(existsAccountNo(receiverAccountNo) == false) {//앞에 ! 연산자 사용가능
  			throw new AccountNotFoundException("이체받을 계좌가 존재하지 않습니다");
  		}
  		//송금자 비밀번호, 계좌 확인
  		int balance = findBalanceByAccountNo(senderAccountNo, password);
  		if(balance < money) {//잔액확인
  			throw new InsufficientBalanceException("잔액 부족으로 이체할 수 없습니다");
  		}
  		...
  		try {
  			con = getConnection();
  			//트랜잭션 제어를 위해 수동커밋모드로 설정
  			con.setAutoCommit(false);
  			//송금자 계좌에서 출금
  			String withdrawSql = "update account set balance = balance - ? where account_no = ?";
              ...
  			//예금주의 계좌에 입금
  			String depositSql = "update account set balance = balance + ? where account_no = ?";
  			...
  			con.commit();
  		} catch(Exception e) {//예외 발생시 rollback을 통해 원상태로 복귀 & 예외 전파
  			//문제가 발생하면 작업을 취소하고 원상태로 되돌린다
  			con.rollback();
  			System.out.println("계좌이체 transaction 내에서 예외 발생");
  			throw e;
              ...
  	}
  ```

####    3 - 3. 7 가장 높은 잔액을 보유한 계좌를 조회하는 메서드(findHighestBalanceAccount())

- 이 메서드는 sql 구문 연습을 해보기 위한 메서드라고 할 수 있다. 해당 sql은 subquery임을 유의하자. 

- 또한 가장 높은 잔액을 보유한 계좌가 1개 뿐만이 아닌 여러 개일 수 있기 때문에 다수의 객체를 담기 위해 ArrayList 자료구조체를 사용한다. 

- 코드는 지금까지 작성해왔던 코드들과 유사하므로 sql 구문만 확인해본다.

  ```java
  public ArrayList<AccountVO> findHighestBalanceAccount() throws SQLException {
  		ArrayList<AccountVO> list = new ArrayList<AccountVO>();
      ...
          StringBuilder sql = new StringBuilder("select account_no, name, balance ");
  			sql.append("from account ");
  			sql.append("where balance = (select max(balance) from account)");
      ...
  }
  ```

- 잔액의 값이 최고 잔액일 때, 즉 최고 잔액을 따로 구한 다음 where 조건절 안에 넣어줘야 한다. 

  

  > ## 4.  테스트 

- 가장 핵심적이였던 계좌이체 기능을 확인해본다면 다음과 같이 이체가 이루어 진 것을 확인해 볼 수 있다

  ![image](https://user-images.githubusercontent.com/88620416/132945421-272c7d1c-e6c0-49b0-ad2f-600bab473adf.png)

- 아래는 DB에서도 변경된 값을 확인한 것이다. 

![image](https://user-images.githubusercontent.com/88620416/132945378-d0cbd4ab-4210-4c2d-9f47-5797f17c2ae0.png)

