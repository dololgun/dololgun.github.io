# Entity Mapping

## @Entity
name은 jpa에서 사용할 엔티티의 이름을 지정한다. 데이터베이스의 테이블 이름이 아니다. 

## @Table
name : entity와 매핑할 테이블명을 지정한다.
catalog : catalog기능이 있는 데이터베이스에서 사용
schema :schema 기능이 있는 데이터베이스에서 사용
uniqueConstraints = {@UniqueConstraint( name = "", columnNames = {} ) } - 어플리케이션에 실행에는 영향을 주지 않지만 ddl을 정의하기 위해서 사용되는 어노테이션도 있다. 

## @Id
엔터티의 식별자 역할을 하는 칼럼에 지정한다. Id칼럼은 당연히 테이블의 PK이다. 

## @Column
자바는 camel 방식의 필드명을 사용하고 db는 언더스코어 방식의 필드명을 사용하는 것이 관례이다. 이런 경우 모든 entity의 필드에 DB의 칼럼명을 매핑해야 하는 작업이 필요하다. 이렇게 직접 하는 것 보다 hibernamte.ejb.naming_strategy 옵션의 "org.hibernate.cfg.ImprovedNamingStrategy"를 사용한다. 이 클래스는 테이블명이나 칼럼명이 생략되면 자바의 카멜 표기법을 테이블의 언더스코어 표기업으로 매핑한다.

#### name
매핑되는 테이블의 칼럼명

#### nullable
칼럼의 값이 null인지 여부 

#### length
칼럼의 길이 

## @Enumerated
Enum 타입을 필드에 지정한다. 

## @Temporal
자바의 날짜 타입을 사용할 때 지정한다. 

## @Lob
데이터베이스의 Clob, Blob을 사용할 때는 Lob어노테이션을 지정한다.