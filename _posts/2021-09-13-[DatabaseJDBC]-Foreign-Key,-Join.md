---
title: "[Database/JDBC] Foreign Key 와 Join"
excerpt:

categories:
  - Database
tags:
  [Database, jave, JDBC]

toc: true
toc_sticky: true

date: 2021-09-13
last_modified_at: 2021-09-13

---



> ## 1. Foreign Key(외래키)와 Join 의 필요성

| 사원번호 | 사원명 | 전화번호 | 주소 | 부서명   | 부서지역 | 부서Tel |
| -------- | ------ | -------- | ---- | -------- | -------- | :-----: |
| 1        | 홍길동 | 010-...  | 성남 | 전략기획 | 광화문   |   02)   |
| 2        | 김영희 | 017-...  | 수원 | 연구개발 | 판교     |  031)   |
| 3        | 김철수 | 011-...  | 포항 | 전략기획 | 광화문   |   02)   |
| 4        | 이제훈 | 019...   | 강북 | 연구개발 | 성남     |  031)   |

다음과 같은 데이터가 있다고 생각해보자. 

사원번호의 정보(사원번호, 사원명, 전화번호, 주소)는 개인정보라서 4개의 컬럼 값이 모두 같을 수는 없다. (특히 전화번호의 경우 같을 수 없음) 하지만 부서와 관련된 정보인 부서명, 부서지역, 부서Tel은 사원마다 중복될 가능성이 매우 큰 데이터이므로 다수의 사원이 같은 정보를 가지는 경우가 발생할 수 있다. (한 부서 내에 여러 직원들이 있을 수 있기 때문)

위와 같이 한 테이블 내에 사원정보와 부서정보를 같이 담게 되면 부서정보의 경우 필요없는 중복이 생기게 된다. 표에서 사원번호 1, 3 의 '전략기획' 부서정보가 중복되는 것처럼 말이다. 
이러한 불필요한 중복을 제거하여 메모리의 낭비를 최소화하고, 데이터를 효율적으로 관리하기 위해 사원정보만담은 사원테이블, 부서정보만 담은 부서테이블로 분리하는 작업이 필요하다.(정규화)

그렇다면 분리를 하고나서 사원에 해당하는 부서정보를 얻기 위해 어떻게 접근해야 할까? 

![erd4](https://user-images.githubusercontent.com/88620416/133065970-ab0e0167-74fa-4520-a2c8-4dd5ed27a879.png)

바로 '***Foreign Key(외래키)***'다.
외래키를 사용하면 중복을 제거함(데이터베이스 정규화 : 중복을 제거한 데이터들을 나타내는 것은 정규화의 첫걸음)으로써 데이터를 효율적으로 관리하게 되고 이것은 무분별한 서버의 증설을 줄일 수 있다.

여기서 외래키는 사원테이블의 부서번호, 신체정보의 사원번호 속성이다. (컬럼명 옆에 FK로 표현)

부서테이블의 기본키를 사원테이블에서 외래키로 가지게 됨으로써 고유한 값인 부서번호(외래키)를 통해 부서테이블에 접근할 수 있게 된다.(부서번호는 부서테이블의 기본키이기 때문) 테이블을 분리하여 데이터의 중복 없이, 외래키를 통해 필요할 때만 찾아올 수 있게 되면서 데이터를 효율적으로 관리할 수 있게 되는 것이다. 

실세계의 예시 : 이 말은 중복된 부서 정보를 가지지 않아도 된다는 것인데, 이것을 실세계로 표현하면 창고 증설 비용의 감소 ---> 서버 데이터 자원의 서버 비용을 줄일 수 있게 되는 것이다. 
별도의 테이블에 부서정보가 1번만 저장되기 때문에 메모리 공간을 효율적으로 사용할 수 있게되고, 부서번호의 pk만 가지고 다니면 되기 때문에 가벼워진다.

- Relation 

  - 부모(부서, 신체정보 입장에서의 사원)의 입장에서 관계를 시작한다.
    기준 먼저 잡기 : 부서테이블의 하나의 row가 하나의 부서 , 이 하나의 부서가 사원테이블에 여러 개 들어갈 수 있기 때문에 1대다 관계( 사원이 多, 부서가 1의 '1대다 관계' )

  - 사원의  `|` 표기 : 한 명의 사원은 반드시 하나의 부서에 속한다.
    부서의  `0 , | , <-`  표기 : 다수의 부서가 사원에 속한다.

  - 부서테이블과 사원테이블은 **비식별관계** - 부서의 기본키를 사원의 일반속성으로 가진다.

  - 사원테이블과 신체정보 테이블은 **식별관계** - 사원의 기본키를 신체정보에 그대로 전이 (사원 기본키 = 신체정보 기본키)
    - 사원 테이블에 매번 같이 조회되는 것보다는 필요시에만 조회가 필요할 때 사용한다.
    - 사원테이블의  시력, 몸무게 신장 데이터를 잘 사용하지 않을 때 역시 별도의 테이블(신체정보)로 관리 
    - 신체정보의 사원번호는 기본키이자 외래키가 된다 , 사원번호에 해당하지 않는 키는 신체정보 테이블에 저장 안됨
    - 입사한 사원에 대한 foreign key 조건이 없다면 사원번호가 없는 일반인의 신체정보도 존재할 수 있기 때문 ==> 무결성 위배 ==> foreign key 필요! 

- 정규화 

  테이블을 목적에 따라 분류하여 정리하는 것
  데이터의 중복을 제거하고 부모의 기본키에 해당하지 않는 엉뚱한 데이터가 들어가서 초기화되버림으로 인해 발생하는 문제요소 및 이상현상을 방지하여 데이터의 무결성을 보장한다.

- 외래키라는 제약조건을 만들어 참조 무결성을 보장

Join은 이렇게 나눠진 테이블에서 각각의 값이 필요할 때 사용되는 sql 쿼리문이다. 자세한 내용은 아래에서 살펴보도록 한다.

> ## 2. Foreign Key(외래키)

- ### Foreign Key(외래키) 란?

  ------------------

  -  관계형 데이터베이스에서 외래키(외부키, Foreign Key)는 한 테이블의 필드(속성, attribute) 중 다른 테이블의 행(row)을 식별할 수 있는 키를 말한다.
  - 외래키를 가지고 있는 테이블이 있다면 그 테이블은 자식 테이블이 된다.(상황에 따라 부모, 자식 테이블 둘 다 가능)
  - 부모테이블 : 참조 대상이 되는 테이블
    자식테이블 : 참조하는 테이블 

- ### 사용법 및 제약조건

  ----------------

  - Foreignn Key 제약 조건 : 참조 무결성 보장을 위한 제약조건 , 다른 테이블의 정보를 참조할 때 지정해야 하는 제약조건. 여기서 참조 무결성은 '참조하는데 결함이 없다 = 존재하지 않는 기본키로 참조할 수 없다' 는 뜻이다. 

  - 외래키를 가지고 있는 테이블 생성시 다음과 같이 작성한다.

    CONSTRAINT : 만약 없는 부서번호를 사용한다면 참조하는데 결점이 생기기 때문에 무결성의 원칙에 위배된다(결점이 없는 성질에 위배) ==> 따라서 제약조건인 fk(foreign key) 제약조건을 걸어준다 ==> 부모테이블에 존재하지 않는 값을 입력하려 할 경우 에러 발생하는 제약조건 

    ```sql
    CONSTRAINT 제약조건명 FOREIGN KEY(현재 테이블의 FK가 될 컬럼명) REFERENCE 참조테이블명(컬럼명)
    ```

    ```sql
    CONSTRAINT fk_deptno FOREIGN KEY(deptno) REFERENCES department(deptno)
    ```

  - 참조 대상이 되는 테이블을 ***부모테이블*** (부서)이라 하고, 참조를 하는 테이블을 ***자식테이블*** (사원)이라고 한다.

  - 일반적으로 비식별관계 사용

  - 보통 관계를 부여하자마자 foreign key 생성된다 -   참조시 결점이 없는 성질을 위함.(무결성의 원칙) 

  - 테이블 생성 순서 : 관계 하에 있는 테이블의 경우, 자식테이블에서 부모테이블의 정보(부모테이블의 기본키)를 참조하고 있으므로 반드시 **부모테이블을 먼저 생성**한 뒤, 자식테이블을 생성해야 한다.

    ```sql
    -- 1. 부모테이블 department 생성
    CREATE TABLE department(
    	deptno NUMBER PRIMARY KEY,
    	dname VARCHAR2(100) NOT NULL,
    	loc VARCHAR2(100) NOT NULL,
    	tel VARCHAR2(100) NOT NULL
    )
    
    -- 2. department 참조하는 테이블 생성
    CREATE TABLE k_employee(
    	empno NUMBER PRIMARY KEY,
    	ename VARCHAR2(100) NOT NULL,
    	sal NUMBER NOT NULL,
    	deptno NUMBER NOT NULL,
        -- 외래키 생성
    	CONSTRAINT fk_deptno FOREIGN KEY(deptno) REFERENCES department(deptno)
    )
    
    ```

> ## 3. Join(Basic)

- ### Join SQL 

  -----

  여러 테이블 간의 정보를 행으로 결합하기 위한 SQL

  

- ### Syntax

  -----------

  - 형식은 2가지가 있으며 표현상의 차이일 뿐 내부 동작에서 차이점은 없다.

  ```sql
  형식1> -- 주로 많이 사용
   	SELECT 컬럼명, 컬럼명
   	FROM 테이블명 별칭, 테이블명 별칭
   	WHERE 별칭.컬럼명 = 별칭.컬럼명 	--조인 조건
   			
   형식2>
   	SELECT 컬럼명, 컬럼명
   	FROM 테이블명 별칭
   	INNER JOIN 테이블명 별칭 ON 별칭.컬럼명 = 별칭.컬럼명
  ```

  

  - 외래키와 join을 이용하여 두 테이블의 정보를 가져오는 예

  ```java
  public EmployeeVO findEmployeeByEmpno(int empno) throws SQLException {
  	EmployeeVO employeeVO = null;
  	//db연동 코드 생략
  	try {
  		con = DriverManager.getConnection(url, username, password);
  		/*
  		String sql2 = "select e.empno, e.ename, e.sal, d.deptno, d.dname, d.loc, d.tel "
  					+ "   from k_employee e "
  					+ "   inner join department d "
  					+ "   on e.deptno = d.deptno "
  					+ "   where e.empno = ?";
  			*/
  		StringBuilder sql = new StringBuilder();
  		sql.append("select e.ename, e.sal, d.deptno, d.dname, d.loc, d.tel ");
  		sql.append("from k_employee e, department d ");
  		sql.append("where e.deptno = d.deptno and e.empno = ?");
  		pstmt = con.prepareStatement(sql.toString());
  		pstmt.setInt(1, empno);
  		rs = pstmt.executeQuery();
  		//기본생성자로 set메서드 이용하는 것과 생성자 만들어서 사용하는 방법 2가지가 있다
  		if(rs.next()) {
  			//데이터가 있을 경우 값 생성
  			employeeVO = new EmployeeVO();
  			//employeeVO.setEmpno(rs.getInt(1));
  			employeeVO.setEmpno(empno);
  			employeeVO.setEname(rs.getString(1));
  			employeeVO.setSal(rs.getInt(2));
  			employeeVO.setDepartmentVO(new DepartmentVO(rs.getInt(3), rs.getString(4), rs.getString(5), rs.getString(6)));
  				
  		}
  			
  	}finally {
  		closeAll(rs, pstmt, con);
  	}
  	return employeeVO;
  }
  ```

  - findEmployeeByEmpno(int empno)의 리턴값은 하나의 값만 반환이 된다. 만약 외래키를 사용하지 않는 상태에서 사원이 부서번호(deptno)만 가지고 있다면 부서의 다른 데이터가 아니라 부서번호만 가지고 있기 때문에 map, list, array등을 통해 다른 데이터들에 접근해야 한다 

    이렇게 되면 코드가 복잡해지고 번거롭기 때문에 foreign key Constraint 을 이용하여 부서 객체에 접근하여 값을 반환한다.

------

- Reference
  - https://ko.wikipedia.org/wiki/%EC%99%B8%EB%9E%98_%ED%82%A4
