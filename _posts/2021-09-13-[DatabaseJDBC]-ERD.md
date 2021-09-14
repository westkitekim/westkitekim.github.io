---
title: "[Database/JDBC] ERD(Entity Relationship Diagram)"
excerpt:

categories: 
  - Database
tags: 
   [Database, java, JDBC]

toc: true
toc_sticky: true

date: 2021-09-13
last_modified_at: 2021-09-13 

---

# ERD(Entity Relationship Diagram)

------

> ## 1. ERD(Entity Relationship Diaram)란?

- ERD(Entitty Relationship Diagram) 은 개체-관계-모델링, 즉 관계형 데이터베이스 설계를 위한 다이어그램을 말한다. 영어 약자 그대로 **'존재하고 있는 것(Entity)들의 관계(Relationship)을 나타낸 도표(Diagram)'** 이다.

- 데이터의 요구분석사항을 도식화 한 관계로 나타내는 것이라고 할 수 있다. 요구사항을 도식화했다는 점에서는 UML과 동일하다. 하지만 UML은 Application에서의 모델링 기법이며  ERD는 데이터베이스를 이용한 모델링 기법이라는 차이점이 있다. 

  ![erd4](https://user-images.githubusercontent.com/88620416/133065970-ab0e0167-74fa-4520-a2c8-4dd5ed27a879.png)

  

- Entity : 개체라는 뜻으로, 관련있는 속성들이 모여 생성된 의미있는 하나의 정보단위이다. 위 사진에서는 데이터들의 관계를 나타낸 도표 즉,  신체정보, 사원, 부서 테이블을 말한다. 

- **논리 데이터 모델링 & 물리 데이터 모델링**

  1. 논리 데이터 모델링 
     - 분석설계 초기에 많이 사용
     - 논리적인 데이터 관리 및 관계를 정의한 모델. 전체 업무 범위와 업무 구성요소를 정의하고 확인할 수 있다.
     - 주로 한글로 사용
  2. 물리 데이터 모델링
     - Application 에서 실제 DB에 맞도록 최적화 시킨 모델링 방법
     - 논리 데이터 모델을 DBMS 특성에 맞게 구체화 시킨 모델을 말한다.
     - 주로 영어로 사용

- **식별관계(identified relationship) & 비식별관계(non-identified relationship)**

  1. 식별관계
     - 부모 테이블의 기본키 혹은 복합키가 자식 테이블의 기본키 혹은 복합키의 구성원으로 전이되는 관계 (ex. 사원과 신체정보 테이블)
     - Relationship 에서 실선으로 표시 , 기본키(PK)를 서로 공유(각 테이블의 기본키가 같다)
  2. 비식별관계
     - 부모 테이블의 기본키 혹은 복합키가 자식 테이블의 일반속성으로 전이되는 관계 (ex. 부서워 사원 테이블)
     - 부모테이블의 기본키(PK)가 자식테이블의 일반속성으로 들어가는 관계(기본키는 다르지만 부모테이블의 기본키를 일반속성으로 가지고 있음)
     - Relationship 에서 점선으로 표시

> ## 2. ERD Tools

- ERD를 만들기 위해 다음과 같이 무료 및 유료의 다양한 도구들이 있다

- 상용 : 마이크로디자이너, eXERD, CA ERwin Data Modeler, DB Visual ARCHITECT, OmniGraffle... etc

- 무료 소프트웨어 : DBDesignr-Fork, 다이어, 페렛, 카이비오, MySQL 웍벤치... etc

  cf) 다양한 도구들이 있으며 여기서는 ERwin를 사용하였다. 

> ## 3.  Setting

- File -> New -> Logical/Physical 버튼 선택 -> 작성 시엔 Logical 데이터 모델링부터 구성
- ERWIN 환경 설정 
  메뉴 Model - Model Properties... 선택 -> 세 번째 Notation탭에서 Logical Notation Physical Notation영역 모두 IE옵션 버튼을 선택 
- 데이터 타입 변경 : 메뉴 Model - Model Properties... 선택 -> 네 번째 Defaults 탭에서 Default Null Option 에서 2개 모두 Not Null, Default Datatype 에서 Logical, Physical 모두 VARCHAR2(100) 으로 설정 (편의를 위한 설정일 뿐 나중에 다시 변경해서 사용 가능)
- 데이터 타입 명시 : 메뉴 Format - Table Display - Column Datatype 체크하면 데이터 타입을 명시해준다.
- 이미지 추출 : 메뉴 Tools - Report Template Builder - Report Builder - Report Templates - Picture 선택후 화살표 클릭 -> File -run - browser 뜨면 이미지 다운 가능
- HTML 안나올때는 경로 변경 : Report Template Builder - Edit -  Properties - Export탭 -  Generated File 위치변경
- 테이블 복사 과정은 ctrl + c, ctrl + v로 동일하나 복사된 개체가 원래 객체에 바로 위에 덮어져있기 때문에 끌어서 빼준다.

> ## 4.  ERD 표기법

- 보통 테이블 하나를 표기하는 방법을 다음과 같이 작성한다. 
- Tab key로 이동하면서 최상단에 개체의 이름, 기본키, 일반 속성 영역 순으로 작성한다. 

<img src="https://user-images.githubusercontent.com/88620416/133069617-d287dd0b-cec9-4bac-a702-390173993cf9.png" alt="erd5" style="zoom: 67%;" />

- Entity 간의 관계 표기법

![img](https://user-images.githubusercontent.com/88620416/133066902-c8b3d1b9-c9d9-4057-8c3f-cd7aa2948596.gif)

- 위 표기법을 참고하여 ERD 예제와 함께 확인해보자.

![erd4](https://user-images.githubusercontent.com/88620416/133065970-ab0e0167-74fa-4520-a2c8-4dd5ed27a879.png)

- 위의 ERD에서 사원테이블이 부서테이블을 참조하고 있기 때문에 부모 테이블은 부서테이블, 자식 테이블은 사원테이블이 된다.
- 표기법 사진을 참고하여 부서테이블이 A, 사원 테이블이 B라고 한다. 부서테이블의 정보는 사원 테이블에서 여러 번 중복될 수 있기 때문에 |, <-, 0이 모두 사용된 것을 확인할 수 있다. 반면, 신체정보 테이블처럼 1개만으로 구성이 될 때는 | 만 사용한다. 
- 실선은 식별관계, 점선은 비식별관계(상단의 '1. ERD란?' 참고)
- 위 테이블을 이용하여 다음 장에서 Foreign key와 join에 대해 자세히 알아보도록 하자. 

> ## 5. ERD Modeling 주의점

- 데이터 모델링 시 가장 중요한 것은 ***실제 원천 데이터(raw data)***다.
  프로젝트 설계 시 분석설계 단계에 앞서서 실제 데이터를 작성해보지 않고 맞다, 틀리다를 논하는 것은 어불성설이다. 모델링의 정확도를 위해 가장 먼저 실제 데이터를 작성해보자. 



-----------------------

- Reference
  - [[DB\] ERD(Entity-Relationship Diagram) : 네이버 블로그 (naver.com)](https://m.blog.naver.com/PostView.naver?blogId=dktmrorl&logNo=220475357522&proxyReferer=https:%2F%2Fhuskdoll.tistory.com%2Fm%2F541)
  - https://ko.wikipedia.org/wiki/%EA%B0%9C%EC%B2%B4_(%EC%BB%B4%ED%93%A8%ED%8C%85) 
  - https://mjn5027.tistory.com/43