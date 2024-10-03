# 🎯핵심 키워드 정리
## 외래키
- 관계형 데이터베이스에서 두 테이블을 연결해주는 데 사용하는 키 값이다.
- 외래키는 다른 테이블의 기본키 값을 참조한다.
- 이때, 외래키를 가진 테이블을 자식 테이블, 외래키를 제공하는 테이블은 부모 테이블이라고 한다.
- 특징
  - 무결성 유지 : 외래키가 부모의 기본키와 정확히 일치할 때만 삽입/삭제/업데이트 작업이 수행된다.
  - 관계 정의 : 테이블 간의 관계를 정의할 수 있으며, 둘 이상의 테이블에서 검색과 조인을 쉽게 할 수 있다.

## 기본키
- 테이블 내에서 각 행을 고유하게 식별하는 키 값이다.
- 기본키를 사용하면 인덱스 방식이 적용되므로 테이블에서 특정 행을 빠르게 찾을 수 있다.
  - 인덱스? : 데이터베이스 테이블의 검색 성능을 높여주는 자료구조를 의미 (해시 테이블 등등..)
- 특징
  - 고유성 : 기본키의 값은 테이블 내에서 중복되지 않는다.
  - 무결성 : 키본키의 값은 NULL이 될 수 없다.
```mysql-sql
예시)

CREATE TABLE parent (
    parent_id INT NOT NULL,
    name VARCHAR(50),
    PRIMARY KEY (id)
);

CREATE TABLE child (
    child_id INT NOT NULL,
    name VARCHAR(50),
    PRIMARY KEY (id),
    FOREIGN KEY (parent_id) REFERENCES parent(parent_id)
);
```

## ER 다이어그램
- ERD(Entity-Relation Diagram)는 개체(Entity)와 관계(Relation)를 이용하여 데이터베이스 구조를 나타낸 다이어그램이다.
- ERD 표기법
    - 개체(Entity)
      - 단독으로 존재하는 객체를 의미한다.
      - 데이터베이스 테이블이 개체로 정의될 수 있다.
      - 사각형으로 표현한다.
    - 속성(Attribute)
      - 개체가 가지고 있는 속성을 의미한다.
      - 원으로 표현한다.
      - 기본키는 추가로 밑줄을 긋는다.
    - 관계(Relationship)
      - 개체 간의 관계를 의미한다.
      - 마름모로 표현한다.
    - 관계성 표현
      - 새발 표기법으로 연관관계를 표현한다.

## 복합키
- 두 개 이상의 컬럼을 기본키로 설정하는 것이다.
- 하나의 컬럼만으로는 고유성을 보장하기 어려울 때 사용한다.
- 장점
  - 무결성 보장 : 데이터의 중복을 방지하여 고유성을 보장한다.
- 단점
  - 외래키 사이드 이펙트 : 자식 테이블이 외래키를 가질 때, 조합된 컬럼을 모두 포함해야 한다.
  - 인덱스 성능 문제 : 조회 시 컬럼의 순서에 따라 인덱싱 성능이 달라질 수 있다. (관계형 DB의 인덱스 알고리즘인 B-Tree 특성)
  > a, b, c 순서로 복합키를 조합한 경우,
  > <br>where a = 1 and b = 2 : 인덱스 스캔 적용(O)
  > <br>where b = 2 : 인덱스 스캔 적용(X)
- 조회 성능 향상을 위해 카디널리티(중복도)를 고려하여 설계한다.

## 연관관계
- 테이블 간의 관계를 1:1/1:N/N:M 으로 나타낸 것이다.
  - 1:1 
    - 하나의 레코드가 다른 테이블의 레코드 하나와 연결된 것이다.
    - 예시) 사용자는 하나의 전화번호를 갖고, 하나의 번호는 한 명의 사용자만을 가진다.
  - 1:N
    - 하나의 레코드가 다른 테이블의 레코드 여러 개와 연결된 것이다.
    - 예시) 선수는 하나의 팀에 속하고, 하나의 팀은 여러 명의 멤버를 가질 수 있다.
  - N:N
    - 여러 개의 레코드가 다른 테이블의 레코드 여러 개와 연결된 것이다.
    - 예시) 학생은 여러 개의 과목을 수강할 수 있고, 하나의 과목은 여러 개의 학생을 가질 수 있다.

## 정규화
- 데이터베이스 설계에서 중복 최소화/무결성 유지를 위해 데이터를 구조화하는 과정이다.
- 정규화의 단계
  - 제 1정규형
    - 각 컬럼은 하나의 속성을 가져야 한다.
    - 하나의 컬럼은 같은 타입을 가져야 한다.
    - 각 컬럼이 유일한 이름을 가져야 한다.
    - 컬럼의 순서가 상관없어야 한다.
  - 제 2정규형
    - 제 1정규형을 만족해야 한다.
    - '부분적 함수 종속'이 없어야 한다.
      - 특정 컬럼이 복합키의 일부에 종속되어선 안된다는 뜻
  - 제 3정규형
    - 제 2정규형을 만족해야 한다.
    - '이행적 함수 종속'이 없어야 한다.
      - a->b, b->c로 a->c가 될 때 이행 종속성이라고 한다.
  - BCNF
    - 강한 제 3정규형이라고도 한다.
    - 복합키(a, b)->c, c->b가 될 때, 테이블을 분리한다.
- 장점
  - 무결성 보장 : 데이터의 중복을 방지한다.
- 단점
  - 조인 성능 문제 : JOIN 연산의 증가로 성능 문제가 발생할 수 있다.

## 비정규화
- 성능 향상을 위해 의도적으로 데이터를 중복하거나 테이블을 결합하는 것이다.
- 비정규화의 대상
  - 조인을 지나치게 많이 사용하는 경우
  - 읽기 작업이 많이 일어나는 시스템
- 장점
  - 빠른 데이터 조회가 가능하다.
  - 조회 쿼리가 간단해진다.
- 단점
  - 중복으로 저장하므로 일관성이 깨질 수 있다.
  - 중복으로 저장하므로 저장공간도 늘어난다.