# Database

주 참고 서적 : [데이터베이스 개론(2판) - 김연희](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791156644316&orderClick=LAG&Kc=)

### 데이터베이스 시스템

- 스키마

  - 스키마(릴레이션 스키마)
    - 데이터 구조와 제약 조건을 명시
    - 속성, 개체, 관계에 대한 정의와 이들이 유지해야할 제약 조건 기술
      - 속성 : 개체의 특성
      - 개체 : 속성의 집합
      - 관계 : 개체 사이의 관계
    - 인스턴스
      - 스키마에 실제로 저장된 값

  - 분류 (3단계 데이터베이스 구조)

    ![](https://mblogthumb-phinf.pstatic.net/20131218_87/hjs4827_1387346999490rQh3c_PNG/3-2.png?type=w2)

    - 외부 스키마
      - 개별 사용자 관점, 여러 개 존재 가능
      - 전체 데이터베이스 중 특정 사용자가 관심을 가지는 일부분
    - 개념 스키마
      - 조직 전체의 관점, 하나 존재
      - 모든 사용자에게 필요한 데이터를 통합한 데이터베이스. 데이터베이스의 전체적인 논리적 구조
      - 일반적으로 스키마로 불림
    - 내부 스키마
      - 물리적 저장 장치 관점, 하나 존재
      - 데이터베이스를 구성하는 레코드의 크기, 레코드를 구성하는 필드의 크기등을 기술
  - 데이터 독립성 (3단계 데이터베이스의 목적)
    - 하위 스키마가 변하더라도 상위에는 영향을 끼치지 않는 성질
    - 변경시 상위와 하위 스키마의 매핑 정보만 바꿔주면 된다
      - 매핑 : 상위 스키마와 하위 스키마 사이의 대응 관계
    - 분류
      - 논리적 데이터 독립성 : 개념 스키마 변경되어도 외부 스키마 영향X(외부/개념 사상)
      - 물리적 데이터 독립성 : 내부 스키마 변경되어도 개념 스키마 영향X(개념/내부 사상)

- 데이터 언어

  - 데이터 정의어(DDL)
    - 스키마 정의 및 수정 삭제
  - 데이터 조작어(DML)
    - 데이터 CRUD 작업 수행
  - 데이터 제어어(DCL)
    - 내부적으로 필요한 규칙 및 기법 정의
    - 다음의 특성 보장 시키기 위함
      - 무결성 : 유효한 데이터만 저장
      - 보안 : 허가된 사용자만 접근 가능
      - 회복 : 장애 발생시에도 일관성 유지
      - 동시성 : 여러 사용자가 같은 데이터에 동시 접근 가능

<br>

### 데이터 모델링

- 데이터 모델링

  - 현실 세계의 데이터를 컴퓨터 세계의 데이터베이스로 옮기는 과정
    - 개념적 데이터 모델링
    - 논리적 데이터 모델링
  - 구성요소
    - 데이터 구조
    - 연산
    - 제약조건
  
- 개체-관계 모델 (E-R Model)
  - **개념적 모델링을 하기 위해 사용**
    
    - 현실 -> 개념
  -  개체
    - 고유한 것(다른 사물들과 구별)
    - 개체 인스턴스 : 개체의 실제 값
    - ex)병원 : 환자, 의사, 간호사, ...
  - 속성
    - 개체가 가지고 있는 고유의 특성
    - 분류
      - 개수 - 개체 인스턴스 당 몇 개 가질 수 있는지
        - 단일 값 속성
        - 다중 값 속성
      - 의미의 분해 가능성 - 속성이 하나 아니면 여러개의 집합으로 이뤄졌는지
        - 단순 속성
        - 복합 속성
          - 예시 : 생년월일 -> 년, 월, 일로 세분화 가능
      - 기존 값에서 유도
        - 유도 속성
          - 예시 : 할인 가격 -> 정가, 할인율에 의해 결정
      - **NULL 속성** 
        - 아직 결정되지 않거나 모르는 값, 존재하지 않는 값
      - **키 속성** 
        - 개체의 인스턴스를 구분하는 데 사용
          - 예시 : 고객 ID로 고객을 구분
        - **키는 둘 이상의 속성으로 구성되는 경우도 존재**
    - ex) 병원
      - 환자 : 환자명, 병명, 진료과, 담당의사, ...
      - 의사 : 의사명, 진료과목, 직위, 담당환자, ...
      - 간호사 : 간호사명, 소속과, 직위, 담당환자 ...
  - 관계
    - 개체와 개체간의 관계
      - ex) 의사는 환자를 **진료**한다
    - 분류
      - 매핑 카디널리티 
        - 어떤 개체의 인스턴스가 관계를 맺고 있는 상대 개체의 인스턴스 개수
        - 1:1 관계
        - 1:N 관계
        - N:M 관계
      - 참여 특성
        - 개체가 필수 참여
        - 개체가 선택적 참여
      - 종속성(의존)
        - 강한 객체 : 다른 객체의 존재 여부 결정
        - 약한 객체 : 다른 객체에 의해 존재 여부 결정
        - 예시 : 게시글과 댓글의 관계
          - 1: N 관계
          - 강한 객체 : 게시글
          - 약한 객체 : 댓글 -> 게시글이 존재해야 댓글 존재 가능, 게시글이 사라지면 댓글도 사라짐

  - **E-R 다이어그램**

    -  개체(Entity) 관계(Relation) 모델을 도식화 한 것

    - 구성 요소
      - 개체 : 사각형
        - 속성 : 타원
      - 관계 : 마름모
        - 카디널리티
      
    - 예시

      ![](https://blog.kakaocdn.net/dn/SEoJQ/btqvdlvgw6V/K2vRqMkcgHwRdmLv8OyAt0/img.png)

- 논리적 데이터 모델
  - E-R 다이어그램으로 표현된 개념적 구조를 데이터베이스에 저장할 형태(**스키마**)로 표현
    - 데이터베이스 관리 시스템에 의존적
  - 종류
    - **관계 데이터 모델**
    - 계층 데이터 모델
    - 네트워크 데이터 모델

<br>

### 관계 데이터 모델

- 참고 자료
  - https://mangkyu.tistory.com/21

- 관계 데이터 모델의 개념

  <img src = "https://t1.daumcdn.net/cfile/tistory/997A69335A0AD58501">

  - 관계 데이터 모델 

    - 개념 세계 -> 컴퓨터 세계
    - 릴레이션에 개체에 대한 데이터를 담아 데이터베이스 구성

  - 용어 정리

    - 릴레이션 : 하나의 개체에 대한 데이터, 행과 열로 이루어짐
      - 열 -> 속성, 어트리뷰트
      - 행 -> 인스턴스, 튜플
    - 도메인
      - 하나의 속성이 가질 수 있는 모든 값 또는 그 값을 제한 한 것
      - 값을 정하기 어려우면 데이터 타입의 범위 지정 -> ex) CHAR(20)
      - 참고) **속성은 원자값만 가진다**
    - NULL
      - 값이 없음을 나타냄
    - 차수
      - 속성 전체의 개수, 정적
    - 카디널리티
      - 튜플의 전체 개수, 동적

  - 릴레이션과 데이터베이스 구성

    - **릴레이션 : 릴레이션 스키마 + 릴레이션 인스턴스**
      - 릴레이션 스키마 : 릴레이션 명 + 속성의 집합(릴레이션의 첫 행 헤더)
      - 릴레이션 인스턴스 : 릴레이션 내 모든 튜플
    - 데이터베이스 스키마 : 데이터베이스 내의 모든 릴레이션 스키마의 모음
    - 데이터베이스 인스턴스 : 데이터베이스 내의 모든 릴레이션 인스턴스의 모음

  - **릴레이션의 특성(다음 특성들을 만족해야함)**

    - **튜플의 유일성** -> 기준 : 키
    - **튜플의 무순서**
    - **속성의 무순서**
    - **속성의 원자성** -> 속성이 가지는 값은 하나

  - 키

    | 고객아이디 | 고객이름 | 나이 | 등급   | 직업   | 적립금 | 주소            |
    | ---------- | -------- | ---- | ------ | ------ | ------ | --------------- |
    | apple      | 김사과   | 20   | gold   | 학생   | 1000   | 서울시 구로구   |
    | banana     | 반하나   | 25   | vip    | 간호사 | 4500   | 부천시 원미구   |
    | carrot     | 이당근   | 28   | gold   | 교사   | 1500   | 서울시 영등포구 |
    | orange     | 오랜지   | 22   | silver | 학생   | 0      | 서울시 마포구   |

    - 튜플(인스턴스)을 유일하게 구분시켜주는 역할

    - **하나의 속성 또는 여러개의 속성의 집합이 키가 될 수 있음**

    - **특성**

      - **유일성** : 각 튜플들은 유일 -> 각 튜플들은 서로 다른 키를 가짐
      - **최소성** : 꼭 필요한 최소한의 속성으로만 키를 구성

    - 종류

      <img src = "https://bbaejisoo.github.io/static/assets/img/blog/2019-04-01-(1)/2019-04-02-18-55-12.png" style = "float : left; width : 70%;">

      - 슈퍼키 : 유일성을 만족하는 속성 또는 속성들의 집합
        - ex) 고객아이디, (고객아이디, 고객이름), (고객이름, 주소)
      - 후보키 : 유일성 + 최소성을 만족하는 속성 또는 속성들의 집합
        - ex) 고객아이디, (고객이름, 주소)
          - (고객아이디, 고객이름) 은 고객아이디 만으로 유일성을 만족할 수 있으므로 배제 
      - **기본키** : 후보키 중에서 기본으로 사용하기 위해 선택
        - 선택 기준

          - NULL 후보키 배제
            - 고객아이디는 필수 값
            - (고객이름, 주소) 에서 주소는 입력하지 않아도 되는 경우 존재
          - 가변적인 후보키 가급적이면 배제
            - 고객아이디는 변경 불가
            - (고객이름, 주소) 에서 이름, 주소가 바뀔 수 있음
          - 단순한 후보키를 선택 
            - 단순하다 : 데이터 크기가 작거나 속성 값의 개수가 적거나
            - 속성의 개수 : 고객아이디 < (고객이름, 주소)
        - 고객아이디가 기본키로 최종 선정
      - 대체키 : 기본키로 뽑히지 못한 후보키
        - (고객이름, 주소)
      - **외래키** : 다른 릴레이션의 기본키를 **참조**하는 속성 또는 속성들의 집합
        - 릴레이션들 간의 관계 표현(참조)
        - **NULL값을 가질수 있음**
        - 외래키와 기본키의 이름은 달라도 된다, 단 도메인은 같아야 한다
        - 참고) 외래키는 같은 릴레이션을 참조할 수도 있다

- 관계 데이터 모델의 제약

  - 데이터베이스에 정확한(올바른) 데이터가 존재할 수 있게 하기 위함
  - 무결성 제약 조건
    - 무결성 : 데이터에 결함이 없는 상태, 정확하고 유효한 상태
    - 개체 무결성 제약 조건
  
      - 기본키를 구성하는 모든 속성은 NULL 불가 -> 유일성X

    - 참조 무결성 제약 조건

      - 외래키가 참조할 수 없는 값을 가지는 것 불가

        - 외래키가 참조하는 다른 릴레이션에서 튜플의 기본키로 존재하지 않는 값을 참조 불가 

        - 참조할 수 없는 값을 가질 때, 릴레이션 간의 관계 설정 불가하기 때문

      - 예시

        - 데이터베이스 스키마

          - 고객 릴레이션
            - 고객아이디(pk)
            - 고객이름
            - ...

          - 주문 릴레이션

            - 주문번호(p.k)
            - **주문고객(fk)**  = 고객아이디

            - ...

        - 고객 릴레이션에 존재하지 않는 고객이 주문한 경우

          - 주문 릴레이션의 외래키인 주문 고객의 값을 NULL로 설정
        
        - 다음의 경우는 데이터베이스 관리 시스템이 자동으로 지원
        
          - 주문 튜플 삽입 시
            - **반드시 고객 튜플에 존재하는 기본키 중 하나를 참고해야 함**
            - **단, NULL 사용 가능**
          - 고객 튜플 삭제 시
            - **주문 튜플에서 참조하는 튜플 삭제 불가**
          - 고객 튜플 수정 시
            - 기본키가 아닌 다른 속성은 언제든 수정가능
            - 기본키 수정시
              - 주문 튜플의 외래키도 알맞게 변경해야함

<br>

### SQL(MySQL)

> 모든 연산의 기본 단위는 튜플 (하나)임

#### 쿼리 작동 순서

1. **FROM**
2. **ON**
3. **JOIN**
4. **WHERE**
5. **GROUP BY**
6. CUBE | ROLLUP
7. **HAVING**
8. **SELECT**
9. DISTINCT
10. **ORDER BY**
11. TOP

<br>

#### 집계 함수

- COUNT()
- SUM()
- AVG()
- MIN()
- MAX()

<br>

#### GROUP BY / HAVING

- 참고 자료 : https://wkdtjsgur100.github.io/groupby-having/

**GROUP BY**

특정 속성의 값이 같은 튜플을 모아 그룹을 만들고, 그룹별로 검색(중복제거, NULL취급x)

- 장점
  - 집계함수를 사용하기 용이

**HAVING**

그룹화된 결과에 대해 조건절 수행

- WHERE문과 동일
- WHERE문과 다르게 집계함수 사용가능

**예시**

- 컬럼 그룹화

  ```mysql
  SELECT 컬럼 FROM 테이블 GROUP BY 그룹화할 컬럼;
  ```

- 조건 처리 후 컬럼 그룹화

  ```mysql
  SELECT 컬럼 FROM 테이블 WHERE 조건식 GROUP BY 그룹화할 컬럼;
  ```

- 컬럼 그룹화 후 조건 처리

  ```mysql
  SELECT 컬럼 FROM 테이블 GROUP BY 그룹화할 컬럼 HAVING 조건식;
  ```

- 조건 처리 후 그룹화 후에 조건 처리

  ```mysql
  SELECT 컬럼 FROM 테이블 WHERE 조건식 
  GROUP BY 그룹화할 컬럼 HAVING 조건식;
  ```

- ORDER BY가 존재하는 경우

  ```mysql
  SELECT 컬럼 FROM 테이블 WHERE 조건식 
  GROUP BY 그룹화할 컬럼 HAVING 조건식
  ORDER BY 컬럼;
  ```


<br>

#### JOIN

- 참고 자료 : https://yoo-hyeok.tistory.com/98

> 테이블간 공통특성을 통해 여러 테이블에서 가져온 튜플들의 집합

![](https://t1.daumcdn.net/cfile/tistory/99219C345BE91A7E32)

#### SUB QUERY

- 참고자료 : https://mjn5027.tistory.com/51

> 메인 쿼리문 안에 또 다른 쿼리문이 있는 것, 서브 쿼리가 먼저 실행

종류

- SELECT 절 서브쿼리 (스칼라 서브쿼리)
  - 단일 행이나 단일 값으로 리턴되어야 함
- FROM 절 서브쿼리 (inline view 서브쿼리)
  - 하나의 테이블로 리턴되어야 함
- WHERE 절 서브쿼리(중첩 서브쿼리)
  - 단일 행과 다중 행으로 리턴 가능
  - 단일 행
    - `>`, `>=`, `<` , `<=`, `=` 
  - 다중 행
    - IN, ALL, ANY, EXISTS

<br>

### 데이터베이스 설계

> 요구 사항을 분석하여 데이터베이스를 설계하는 과정

#### 과정

1. 요구사항분석 : 요구사항명세서 추출
2. **개념적 설계** : 개념적 스키마(ERD) 추출
3. **논리적 설계** : 논리적 스키마(릴레이션 스키마) 추출
4. 물리적 설계 : 물리적 스키마 추출 (하드웨어, OS 고려)
5. 구현 : SQL문을 작성하여 실제 데이터베이스 생성

#### 개념적 설계

**과정**

1. 개체와 속성 추출 : 명사
2. 관계 추출 : 동사
3. ERD 작성

#### 개념적 설계

릴레이션 변환 규칙 적용, 튜플의 원자성을 지켜야함에 유의

**과정**

1. 모든 개체는 릴레이션으로 변환
2. N:M 관계는 릴레이션으로 변환 : 
   1. n측 릴레이션 기본키와 m측 릴레이션의 기본키를 각각 외래키로 받아와서 두 외래키를 조합하여 기본키로 지정
   2. 외래키 받아오는 건 1과 동일, 새로운 기본키 지정
3. 1:N 관계는 외래키로 변환
   1. 일반적인 1:N 관계는 외래키로 표현 : n측 릴레이션 개체에 1측 릴레이션 개체의 기본키를 외래키로 저장
   2. 약한 개체가 참여하는 1:N 관계는 외래키를 포함해서 기본키로 지정 : 1측 릴레이션 개체가 먼저 존재해야 n측 릴레이션 개체가 존재할 수 있으므로
4. 1:1 관계는 외래키로 변환
   1. 일반적인 1:1 관계는 외래키를 서로 주고 받음
   2. 1:1 관계에 필수로 참여하는 개체의 릴레이션만 외래키를 받음
   3. 모든 개체가 1:1 관계에 필수적으로 참여하면 릴레이션 하나로 통합
5. 다중값 속성은 릴레이션으로 변환

<br>

### 정규화 (초안)

- 정규화의 필요성

  - 데이터 중복으로 인한 삽입, 갱신, 삭제 이상 발생
  - 이를 해결 하고자 정규화 도입

- 함수 종속

  - X -> Y

    - 하나의 속성 집합 X값에 대한 다른 속성 집합 Y의 값이 **항상 하나**

    - Y는 X에 종속
    - X : 결정자, Y : 종속자

- 정규화

  - 하위 정규형은 상위 정규형을 만족함(부분집합)
  - 제1정규형
    - 릴레이션 내 모든 속성의 도메인은 **원자값**을 가짐
  - 제2정규형
    - 릴레이션 내에서 기본키가 아닌 속성이 기본키에 **완전 함수 종속**
    - 부분 함수 종속
      - 완전 함수 종속의 반대, 이 경우 2정규화 진행
      - 기본키를 구성하는 속성의 부분집합이 다른 속성의 결정자가 되는 경우

  - 제3정규형

    - 릴레이션 내에서 기본키아 아닌 속성이 기본키에 대해 **이행적 함수 종속X**
    - 이행적 함수 종속
      - X -> Y 이고 Y -> Z 이면, X -> Z인 경우 Z가 X에 대해 이행적 함수 종속
    - 기본키가 아닌 다른 속성이 또다른 속성의 결정자면 3정규화 진행

  - BCNF (Boyce Codd Normal Form)

    - 릴레이션의 함수 종속 관계에서 **모든 결정자가 후보키**
    - 일반적으로 **기본키가 유일한 결정자면 BCNF 만족**

    - 기본키의 부분 속성 집합이 기본키가 아닌 다른 속성에 종속되면 BCNF를 진행
