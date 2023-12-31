# 1. JPA 소개

## SQL을 직접 다룰 때 발생하는 문제점

자바 애플리케이션은 JDBC API를 사용해 SQL을 데이터베이스에 전달.

#### 코드의 반복

객체를 DB에 CRUD 하려면 SQL 작성, JDBC API사용, 객체 매핑 하는 코드를 반복 작성 해야한다.

#### DB 테이블과 애플리케이션 객체의 구조 차이

데이터 중심의 구조를 가지는 DB와 객체지향 애플리케이션의 객체는 서로 구조가 다르기 때문에 직접 객체를 DB에 저장하거나 조회할 수 없다. 그래서 개발자가 변환 작업을 해야 한다.

#### SQL에 의존적인 개발

요구사항이 변경(연관된 객체 추가 또는 삭제, 컬럼 추가 등)되면 관련된 많은 코드(DAO, SQL등)를 변경해야 한다. 연관된 객체의 내용을 조회 또는 변경 하려면 결국 DAO에서 SQL을 확인해 어떤 SQL이 실행되는지 확인 해야 한다.  
-> 계층 분할이 어렵다.  
-> SQL에 의존적인 개발을 피하기 어렵다.

엔티티(비즈니스 요구사항을 모델링한 객체)와 아주 강한 의존관계를 가지게 된다.  
-> 엔티티를 신뢰할 수 없다.

**☑ 객체를 DB에 저장하고 관리할 때, JPA는 개발자 대신 적절한 SQL을 생성해 DB에 전달한다.**

## 객체와 관계형 DB의 패러다임 불일치

객체지향을 적용한 객체가 상속 받았거나 다른 객체를 참조 하고 있다면 이를 DB에 저장하기 쉽지 않다.  
관계형 데이터베이스는 데이터 중심으로 구조화 되어 있기 때문에 객체지향의 개념을 그대로 적용하기 어렵다.  
객체와 관계형 데이터베이스는 기능과 표현 방법이 다르다.  
-> **객체와 관계형 데이터베이스의 패러다임 불일치 문제**발생. 문제 해결 위해 중간에서 개발자가 많은 시간과 코드를 소비한다.(비용 증가, 복잡성 증가, 유지보수 어려움)

### 패러다임 불일치로 인해 발생하는 문제

#### 상속

- 객체: 상속 사용 | 테이블: 상속 없음

-> JPA를 사용하면 객체를 자바 컬렉션에 저장하듯 저장 가능

#### 연관관계

- 객체: 참조 통해 연관된 객체 조회 | 테이블: 외래 키로 조인을 사용해 연관된 테이블 조회

-> JPA는 참조를 외래 키로 변환해 적절한 SQL문을 DB에 전달

#### 객체 그래프 탐색

객체로 참조를 사용해 연관된 객체를 찾는 것.

- SQL을 직접 다루면 처음 실행하는 SQL에 따라 객체 그래프를 어디까지 탐색할 수 있는지 정해진다.
- 어디까지 탐색할 수 있는지는 직접 SQL을 확인 해야 알 수 있기 때문에 SQL에 의존적인 개발이 된다.
- 조회 위해 상황에 따라 여러 SQL을 작성 해야 한다.

-> JPA는 지연로딩(실제 객체 사용 시점까지 DB 조회 미룸)을 사용

#### 비교

- 객체: 동일성비교(인스턴스 비교), 동등성비교(객체 내부 값 비교) | 테이블: PK로 ROW 비교
- 같은 로우를 조회 해도 객체의 동일성 비교는 실패할 수 있다.

-> JPA는 같은 트랜잭션일 때 같은 객체 조회

**☑ JPA는 패러다임의 불일치 문제를 해결해주고 정교한 객체 모델링을 유지하게 도운다.**

## JPA란

JPA(JAVA PESISTENCE API)는 자바 진영의 ORM(OBJECT-RELATIONAL MAPPING)기술에 대한 표준 명세이다.  
ORM 프레임워크는 객체와 테이블을 매핑해서 패러다임의 불일치 문제를 개발자 대신 해결해준다.

- 객체 측면: 정교한 모델링 가능
- 관계형데이터베이스: 데이터베이스에 맞도록 모델링

-> 관계형 데이터베이스를 사용해도 객체지향 애플리케이션 개발에 집중할 수 있음

☑ **정리**: JPA를 사용해 생산성 향상, 유지보수 용이, 성능 향상(다양한 성능 최적화 제공), 패러다임의 불일치 해결, 데이터 접근 추상화와 벤더 독립성 향상

> 데이터 접근 추상화와 벤더 독립성: JPA는 ORM 기술 표준이다. 그래서 특정 구현 기술에 대한 의존도를 줄이고 다른 구현 기술로 쉽게 이동할 수 있다. (JPA 구현 프레임워크: Hibernate, EclipseLink, ...)
