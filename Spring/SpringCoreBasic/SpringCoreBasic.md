# Spring Core Basic

[**스프링 핵심 원리 - 기본편**](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard) **강의 요약**

<br>

### 집중할 부분

- 이 기술을 왜 만들었는가?
- 이 기술의 핵심 컨셉은?

<br>

### 스프링의 핵심

- 좋은 객체 지향 어플리케이션을 개발하기 위한 프레임워크

<br>

### 객체 지향 프로그래밍

- 객체들의 모임
- 객체들간 메시지를 주고 받음
- **유연**하고 **변경**에 용이 -> **다형성** (부품 갈아 끼우듯이)

<br>

### 다형성 진짜 의미

**Spring의 핵심인 제어의 역전(IoC), 의존관계 주입(DI)은 다형성을 활용해서 구현**

- 핵심 컨셉
  - 이세상을 **역할**과 **구현**으로 구분
  - 객체 설계시 역할(인터페이스)부터 먼저 설계하고 구현
- 비유
  - 운전자와 자동차
    - 자동차가 바뀌어도 운전자는 운전할 수 있다
    - 운전자는 자동자의 역할만 아는 것이지 구체적인 구현체는 몰라도 된다
    - **새로운 자동차가 나와도 운전자는 알 필요가 없다**
  - 로미오와 줄리엣 공연
    - 각 배역은 상대 배역이 누군지에 상관없이 공연 진행
- 장점
  - **클라이언트는 대상의 역할(인터페이스)만 알면 된다**
  - 클라이언트는 **구현 대상의 내부 구조를 몰라도 된다**
  - 클라이언트는 **구현 대상의 내부 구조가 변경 되어도 영향을 받지 않는다**
  - 클라이언트는 **구현 대상 자체를 변경해도 영향을 받지 않는다**
- 단점
  - 역할(인터페이스)이 변하면 구현체 모두에 영향
  - 그래서 설계를 잘 해야함

- 자바 언어의 다형성
  - 오버라이딩된 메소드가 실행
  - 다형성으로 인터페이스를 구현한 객체를 런타임 시점에 유연하게 변경
- 요약
  - **인터페이스를 구현할 인스턴스를 런타임시에 유연하게 변경 -> 확장 가능한 설계**
  - 객체들은 서로 협력 관계
  - **클라이언트를 변경하지 않고 서버의 구현 기능을 유연하게 변경**

<br>

### 좋은 객체 지향 설계 원칙 : SOLID

- SRP(Single Responsibility Principle)
  - 단일 책임 원칙
  - 하나의 클래스는 하나의 책임만 가진다 -> 모호
  - 기준 : 변경
    - 변경이 있을 때 파급이 적으면 잘 구현한 것
  - ex) UI변경, 객체 생성과 사용 분리
- OCP (Open Closed Principle)
  - 확장에는 열려 있고, 변경에는 닫혀 있어야함
    - **기존 코드는 변경하지 않으면서 기능 확장**
  - 다형성 활용
    - 역할과 구현의 분리
    - 인터페이스를 기준으로 확장
  -  현재 강의 단계에서는 클라이언트가 구현 클래스를 직접 선택
    - 구현객체 변경시 클라이언트의 코드를 변경
    - 다형성 활용해도 OCP 깨짐
    - **객체를 생성하고 연관관계를 맺어주는 별도의 설정자 필요** -> IoC 컨테이너의 역할
- LSP (Liskov substitution principle)
  - 리스코프 치환 원칙
  - 다형성에서 하위 클래스는 인터페이스의 규약을 다 지켜야함
  - 단순히 컴파일 통과 하는 것 뿐이 아닌 원래 의도대로 구현
    - ex) 엑셀을 밟는다 -> 앞으로 가야함, 뒤로 가면X
- ISP ( nterface Segregation Principle)
  - 인터페이스 분리 원칙
  - 특정 클라이언트를 위한 인터페이스가 여러 개의 범용 인터페이스 보다 낫다
    - 기능에 맞게 잘 쪼갠다
    - ex) 자동차, 사용자
      - 자동차 인터페이스 : 운전, 정비
      - 사용자 : 운전자, 정비사
  - 인터페이스가 명확해짐, 대체하기 편함
- DIP (Dependency Inversion Principle)

  - 의존(어떤 코드를 알고 있다)관계 역전 원칙
  - 추상화에 의존해야지 구체화에 의존하면 안된다
    - **클라이언트는 역할(인터페이스)에 의존해야지, 구현체에 의존하면 안된다**

  -  현재 강의 단계에서는 클라이언트가 구현 클래스를 직접 선택
    - 인터페이스 뿐만 아니라 구현체도 알고 있는 상태
    - 구현체를 직접 선택 -> DIP 위반
- 요약
  - 다형성 : 객체 지향의 핵심
  - 다형성만 사용할 땐 구현 객체 변경시 클라이언트 코드 수정
  - **다형성만으로 OCP, DIP 못지킴**
  - 뭔가 더 필요 

<br>

### 스프링과 객체 지향

- DI 컨테이너
  - 다형성, OCP, DIP 가능케 함
  - 클라이언트 코드의 변경 없이 기능 확장

<br>

### 스프링 핵심 원리 이해 - 예제 만들기(다형성 활용)

#### 전체 설계

- 회원
  - 가입, 조회
  - 등급 : 일반, VIP
  - 데이터 : 메모리 or 자체 DB or 외부 시스템
- 주문과 할인
  - 주문 : 회원이 함
  - 할인
    - 회원 등급에 따라 다름
    - 고정 할인(1000원) or 퍼센트 할인 가능
- **이처럼 변경 가능성이 있을떄 인터페이스르 통해 구현체를 언제든 갈아 끼울 수 있도록 구현** 

<br>

#### 회원 설계

- 도메인 협력 관계 : 기획자도 같이 봄
- 클래스 다이어그램 : 정적
- 객체 다이어그램 : 동적(런타임시 실제 주입되는 객체 기준)
- 회원 관리 DB가 메모리 or 실제 DB임에 초점

<br>

#### 회원 구현

- member 패키지 생성
- Grade(등급), Member(DTO) 구현

- repo인터페이스 구현 : MemberRepository
  - 가입
  - 아이디로 찾기
- repo구현체 구현 : MemoryMemberRepository
  - ConcurrentHashMap???

- service 인터페이스 구현
- service 구현체 구현

<br>

#### 회원 테스트

- 단위 테스트
  - given, when, then
  - @Test
- 실행 : 회원 가입

<br>

#### 주문과 할인 도메인 설계

마찬가지로...

- 도메인 협력 관계 : 기획자도 같이 봄
- 클래스 다이어그램 : 정적
- 객체 다이어그램 : 동적(런타임시 실제 주입되는 객체 기준)

- 시나리오 
  - 회원은 주문을 한다
    - 주문(Order)
      - 회원id
      - 아이템명
      - 가격
      - 할인 가격
  - 회원 등급에 따라 할인 정책이 다르다 -> 회원 조회 후 할인 정책 적용
    - VIP : 1000원
    - BASIC : 없음
  - 최종적으로 할인을 적용하여 주문 반환

#### 주문과 할인 구현

- discount 패키지 생성 -> 할인 정책
  - DiscountPolicy인터페이스 생성
    - discount(할인)
  - 구현체 : FixDiscountPolicy(정액할인) 생성

- order 패키지 생성

  - Order 클래스 생성

  - OrderService 인터페이스 생성

    - 주문하기

  - OrderServiceImpl 구현체 생성

    - 의존
      - MemberRepository : 회원 정보 조회 후
      - DiscountPolicy : 할인 적용

<br>

#### 주문과 할인 테스트

- 회원 테스트와 동일한 원리
  - 회원 가입
  - 주문
- @SpringBootTest와 단위테스트
  - @SpringBootTest : Spring App구동에 필요한 모든 객체 생성 -> 무거움
  - 단위테스트 : 해당 테스트 실행에 필요한 객체만 생성 -> 가벼움

<br>

#### 문제점

- service부분이 문제
  - 인터페이스와 구현체 모두에 의존 -> OCP, DIP 위반

<br>

### 스프링 핵심 원리 이해 : 객체 지향 원리 적용

#### 새로운 할인 정책 개발

- 기존 정액 할인 -> 정률 할인 으로 변경
- DiscountPolicy 인터페이스를 구현하여 RateDiscountPolicy 생성
- 테스트
  - VIP인 경우
  - VIP가 아닌 경우

 <br>

#### 새로운 할인 정책 적용과 문제점

- 문제점

  - 할인 정책 변경시 클라이언트인 OrderServiceImpl 고쳐야함
  - DIP 위반
    - 인터페이스인 DiscountPolicy뿐만 아니라 구현체인 FixDiscountPolicy, RateDiscountPolicy에도 의존
    - 추상클래스 뿐만 아니라 구체클래스에도 의존
  - OCP 위반
    - 정책 변경시 클라이언트 측에서 코드를 수정해야함

- 해결방안

  - 인터페이스에만 의존하도록 변경

    - 하지만 현재 단계에서는 구현체를 할당하지 못해서 NullPointerException발생

    - **누군가 클라이언트에 인터페이스의 구현 객체를 대신 생성하고 주입해주어야한다**

<br>

#### 관심사의 분리

- 비유
  - 배우는 자신의 배역만 잘 수행하면된다
  - 배우는 상대 배우를 정하지 않는다
  - **상대 배우를 정하는건 공연 기획자이다**

- 책임의 분리

  - 클래스는 자신의 로직을 수행하는 것에만 집중
  - 구현 객체를 생성하고 연결(DI)하는 책임을 가지는 별도의 설정 클래스 필요 

- AppConfig

  - 구현 객체를 생성하고 연결(DI)하는 책임을 가지는 별도의 외부 설정 클래스

  - 역할

    - 실제 동작에 필요한 구현 객체 생성
    - 객체 인스턴스의 참조를 생성자 주입을 통해  연결
    
  - 의의
  
    - 클라이언트는 어떤 구현 객체가 들어올지 모름
    - 외부 설정 클래스에서 구현 객체 생성 후 주입
    - 클라이언트는 의존관계에 대한 고민은 외부에 맡기고 실행에만 집중
    - **DIP 완성**
  
- 테스트

  - @BeforeEact

    - 각 테스트를 실행하기 전에 실행
    - AppConfig를 활용하여 객체 주입

<br>

#### AppConfig 리팩토링

- AppConfig 중복 제거 및 역할에 따른 구현이 잘 드러나게 수정

  - MemberRepository, DiscountPolicy 반환하는 메소드 생성

  - 전체 어플리케이션 구성 빠르게 파악 가능

<br>

#### 새로운 구조와 할인 정책 적용

- 애플리케이션의 관심사 구분
  - 구성 영역 : 객체 생성, 연결
  - 사용 영역 : 로직 수행
- **구현 객체 변경 시 구성 영역인 AppConfig만 수정하면 된다**

<br>

#### SOLID 적용

- SRP

  - 애플리케이션의 관심사 구분
    - 구성 영역
    - 사용 영역

- DIP

  - 구체화가 아닌 추상화(인터페이스)에만 의존
  - 설정 클레스(AppConfig)로 실현함

- OCP

  - 구성 영역만 수정하여 어떤 구현 객체를 주입할지 결정

  - 결과적으로 사용 영역(클라이언트) 코드 수정X

<br>

#### IoC, DI 컨테이너

- IoC : 제어의 역전
  - 프로그램의 제어 흐름을 직접 제어하는 것이 아니고 외부에서 관리
  - 제어 : 객체를 생성하고 관리하는것
    - AppConfig가 OrderService구현에 필요한 인터페이스들의 구현체를 생성하고 연결시켜준다
    - OrderService는 어떤 구현체가 들어올지 모르고 역할(인터페이스)에만 의존하고 있음
  -  프레임워크 vs 라이브러리
    - 프레임워크 : 제어의 역전 적용, 프레임워크에 종속되어 작업 수행
    - 라이브러리 : 개발자가 필요한 라이브러리를 직접 호출하여 작업 수행

- 의존 관계 주입

  - 정적인 클래스 의존 관계와 동적인 객체 의존 관계를 분리하여 접근
    - OrderServiceImpl은 DiscountPolicy 인터페이스에 의존
    - DiscountPolicy 인터페이스에 어떤 구현 객체가 사용될지는 모름

  - 정적인 클래스 의존 관계
    - 역할(인터페이스)만 확인 가능
    - 실제로 어떤 구현 객체가 주입되는지 알 수 없음
  - 동적인 객체 인스턴스 의존 관계
    - 런타임시에 실제 어떤 객체가 주입되는지 알 수 있음
    - **의존 관계 주입** 
      - 런타임 시에 외부에서  실제 구현 객체를 생성하여 의존관계 연결
      - 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 의존 관계만 쉽게 변경할 수 있다
      - 어플리케이션코드를 수정하지 않고, 외부 설정 파일만 변경

- IoC 컨테이너, DI 컨테이너

  - AppConfig 처럼 객체를 생성해주고 의존관계를 연결

<br>

#### Spring으로 전환하기

- ApplicationContext : 스프링 컨테이너 
- @Configuration
  - 스프링 컨테이너는 @Configuration이 붙은 클래스를 설정 정보로 활용
  - 여기서 @Bean이 붙은  메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록
    - 메소드명을 이름으로 해서 bean이 등록
  - Bean : 스프링 컨테이너에 등록된 객체
- 스프링 컨테이너에 등록된 bean 호출
  - getBean()

<br>

### 스프링 컨테이너와 스프링 빈

#### 스프링 컨테이너 생성

- 스프링 컨테이너의 역할

  - 빈 생성(관리)
  - 의존 관계 주입

- Application Context

  - 스프링 컨테이너
  - 인터페이스
    - 구현 객체 필요
      - Annotatiion
      - XML

  - 빈 생성 과정
    - 파라미터로 설정 정보 클래스(@Configuration) 받음
    - @Bean 어노테이션이 있는 메소드 탐색하여 빈 등록
    - 스프링 컨테이너 구조
      - Map 구조
        - key : 스프링 빈 이름(메서드명)
        - value : 객체
      - **Map 구조라서 빈 이름은 중복 불가**
    - 의존 관계 설정
      - 스프링 빈 들간의 의존 관계 설정

<br>

#### 컨테이너에 등록된 모든 빈 조회

- 모든 빈 출력
  - ac.getBeanDefinitionNames() : 컨테이너에 등록된 모든 빈의 이름 조회
  - ac.getBean() : 빈 이름으로 빈 객체 조회 (Object 타입)
- 애플리케이션 빈 출력
  -  ac.getBeanDefinition() : 빈 이름으로 빈의 메타데이터 조회 
  - getRole()
    - ROLE_APPLICATION : 사용자가 정의한 빈
    - ROLE_INFRASTRUCTURE : 스프링 내부에서 사용하는 빈

<br>

#### 스프링 빈 조회 - 기본

- 빈 조회
  - ac.getBean("빈 이름", 타입.class)
  - ac.getBean(타입.class) -> 동일한 타입이 2개일 때 유의
  - 예외
    - 조회하려는 빈이 없는 경우 발생
    - NoSuchBeanDefinitionException 발생
  - 구체 타입으로 조회시 유연성 떨어짐 -> 역할에 의존해야지...

<br>

#### 스프링 빈 조회 - 동일한 타입이 둘 이상

- 동일한 타입의 스프링 빈이 둘 이상일 때 타입 조회 시 오류 발생

  - NoUniqueBeanDefinitionException
  - 해결 :  빈 이름으로 조회

- 해당 타입의 모든 빈 조회

  - ac.getBeansOfType() : `Map<String, Type>`

<br>

#### 스프링 빈 조회 - 상속 관계

- 부모 타입으로 조회시, 자식 타입도 모두 조회
- Object 타입으로 조회시 모든 스프링 빈 조회

- 조회 테스트
  - 자식 2개 이상일 때 부모타입으로 조회 
  - 자식 2개 이상일 때 빈 이름으로 조회
  - 부모 타입으로 모두 조회 -> ac.getBeansOfType()
  - Object타입으로 모두 조회 -> ac.getBeansOfType()

<br>

#### BeanFactory와 ApplicationContext

- 둘다 스프링 컨네이너 이다
  - BeanFactory
    - 스프링 컨테이너의 최상위 클래스
    - 빈을 조회하고 관리
  - ApplicationContext
    - BeanFactory를 상속
    - 다양한 부가 기능 제공
      - 국제화
      - 환경변수
      - 애플리케이션 이벤트
      - 편리한 리소스 조회

<br>

#### 다양한 설정 형식 지원 - 자바 코드, XML

- ApplicationContext에서 지원하는 설정 정보 형식
  - AnnotationConfig... : 자바 설정 파일
  - GenericXml.... : xml 파일
  - 다양한 형식에서 따라 **유연**하게 스프링 컨테이너를 생성할 수 있다 -> **역할과 구현을 분리**

<br>

#### Spring Bean 설정 메타 정보 - BeanDefinition

- BeanDefinition 
  - 다양한 설정 형식 지원해 줄 수 있는 배경
    - 자바, xml 모두 각각의 BeanDefinitionReader가 설정 클래스를 잀어서 BeanDefinition생성
    - 이렇게 생성된 BeanDefinition을 기반으로 스프링 빈을 생성
    - ApplicationContext는 BeanDefinition에만 의존 -> 자바인지 xml 기반인지 알 필요 없음

<br>

### 싱글톤 컨테이너

#### 웹애플리케이션과 싱글톤

- 스프링없는 순수한 DI 컨테이너 테스트

  - 고객의 요청이 있을떄마다 필요한 객체를 생성한다면 메모리 낭비가 심하다
  - 해결책 -> 하나의 객체만 만들어서 공유 (싱글톤)

<br>

#### 싱글톤 패턴

- 싱글톤 패턴
  - 인스턴스가 단 하나만 생성되는 것 보장
  - 객체를 2개 이상 못 만들게 함
- 구현방법
  - static 영역에 instance 생성
  - 인스턴스 호출시 getInstance() 메서드 사용
  - **private 생성자 사용** ->외부에서 new 키워드로 객체 생성 불가
- 스프링 컨테이너
  - **기본적으로 빈을 싱글톤으로 관리**
- 테스트
  - isSameAs() : 주소가 같다
  - isEqualTo() : 값이 같다

- 단점
  - 지저분한 코드
  - 유연성 떨어짐 
    - private 생성자 : 상속 불가
    - 구체화에 의존 -> OCP, DIP 위배

<br>

#### 싱글톤 컨테이너

- 스프링 컨테이너는 **기본적으로** 싱글톤 컨테이너

  - 싱글톤 패턴의 문제점 해결해줌

  - 같은 빈 조회시 주소가 같음
    - 고객 요청시 마다 객체 생성X
    - 이미 만들어진 객체 리턴

<br>

#### 싱글톤 방식의 주의점

- 무상태(stateless)로 설계
  - 싱글톤은 인스턴스 객체를 하나만 공유하기 때문
  - **필드에 공유값(상태를 유지하는 값)을 설정하면 큰 장애 발생 가능**
  - 필드 대신에 공유되지 않는 지역변수, 파라미터, ThreadLocal 사용

<br>

#### @Configuration과 싱글톤

- @Bean을 통해 빈 생성하는 과정

  - @Bean이 있는 메서드 하나씩 호출

  - 예제

    - memberService와 orderService 빈 생성
      - 공통적으로 memberRepository 호출 -> new MemoryMemberRepository() 생성

    - 싱글톤 깨지는거 아님? -> 안깨짐

- 테스트

  - 싱글톤인지 확인

    -  memberService와 orderService의 memberRepository가 같은지 확인

  - 빈 생성시 호출 횟수 확인

    - 예상 : memberRepository 3번 호출

      ```java
      // call AppConfig.memberService
      // call AppConfig.memberRepository
      // call AppConfig.memberRepository
      // call AppConfig.orderService
      // call AppConfig.memberRepository
      ```

    - 실제 : 각각 한 번씩만 호출

      ```java
      // call AppConfig.memberService
      // call AppConfig.memberRepository
      // call AppConfig.orderService
      ```

<br>

#### @Configuration과 바이트코드 조작의 마법

- 설정 클래스 확인

  - AppConfig 클래스로 확인해 보자

    - @Configuration이 있으므로
    - AppConfig를 스프링 컨테이너에 등록
    - 스프링 컨테이너에서 AppConfig 빈 호출 하여 클래스 확인 (getClass())

  - 결과

    - `class hello.core.AppConfig$$EnhancerBySpringCGLIB$$80ab813f`

    - `$$EnhancerBySpringCGLIB$$80ab813f` -> ㅁ...뭐고...?

- CGLIB

  - 바이트코드 조작 라이브러리
  - 원래의 설정 클래스(AppConfig)를 상속한 클래스 생성(AppConfig@CGLIB)
  - 이 상속한 클래스(일종의 복사본)로 싱글톤이 보장되게 함
    - 상속했기 때문에 getBean()으로 조회할 수 있음

  - 예상 로직
    - 해당 객체가 이미 스프링 빈으로 등록
      - O : 스프링 컨테이너에서 찾아서 반환
      - X : 기존 로직 호출 해서 객체 생성 후 스프링 컨테이너에 등록

- @Configuration이 없다면?
  - 스프링 빈으로 등록은 다 됨
  - 하지만 @Bean의 싱글톤을 보장하지 않는다
  - 스프링 컨테이너에서 관리되는 빈이 사용되지 않는다
    - 해결 
      - @Autowired : 스프링 빈으로 등록된 객체 조회 

<br>

### 컴포넌트 스캔

#### 컴포넌트 스캔과 의존 관계 자동 주입 시작하기

- 컴포넌트 스캔과 의존 관계 자동 주입 과정
  - 스프링 빈 등록
    - @ComponentScan
      - @Component가 붙은 모든 클래스를 스프링 빈으로 등록
      - @Component가 붙은 클래스의 이름을 스프링 빈의 이름으로 등록 (맨 앞글자만 소문자) 
  - 의존 관계 주입
    - @Autowired 
      - 의존 관계 자동 주입
      - **타입을 기준**으로 의존 관계 주입 == `getBean(xxx.class)`

<br>

#### 탐색 위치와 기본 스캔 대상

- 컴포넌트 스캔의 탐색
  - basePackage
    - basePackage를 기준으로 시작하여 하위 패키지 모두 탐색
    - **기본값 : @ComponentScan이 있는 클래스의 패키지**
    - 참고) basePackageClass :  
  - 권장 방법
    - 메인 설정 정보는 프로젝트의 시작 루트 위치에 두는 게 좋음
      - 예시) @SpringBootApplication -> @ComponentScan 있음

- 컴포넌트 스캔 기본 대상
  - @Component
  - @Contoller, @RestController : 스프링 MVC Controller
  - @Service : 비즈니스 로직
    - 비즈니스 로직이 시작될 떄 transaction 시작....
  - @Repository :  스프링 데이터 접근 계층
  - @Configuration : 설정 정보
- 참고) Annotation
  - 애노테이션은 자바에서 지원하는 기능 아님 
  - 스프링에서 지원하는 기능

<br>

#### 필터

- 적용 방법
  - `includeFilter` : 컴포넌트 스캔 대상에서 추가
  - `excludeFilter` : 컴포넌트 스캔 대상에서 제외

- 옵션
  - ANNOTATION : 기본값, 애노테이션을 인식
  - ASSIGNABLE : 지정한 타입과 자식 타입을 인식 -> 클래스 직접 설정
  - ...

- 권장 사항
  - 걍 디폴트 설정 대로 쓰는게 좋다...

<br>

#### 중복 등록과 충돌

- 상황
  - 자동 빈 등록 vs 자동 빈 등록
    - ConflictingBeanDefinitionException 발생
    - 발생 확률 거의 없음
  - 수동 빈 등록 vs 자동 빈 등록
    - 수동 빈 등록이 우선권을 가짐
      - log에서 `Overriding bean definition...` 발생
    - 스프링 부트에서는 버그 방지를 위해 충돌 발생 시 디폴트로 에러 발생하게 해놓음
      - 그럼에도 쓰고 싶다 -> `spring.main.allow-bean-definition-overriding=true`

<br>

### 의존 관계 자동 주입

#### 다양한 의존 관계 주입 방법

- 복습) 스프링 컨테이너의 라이프 사이클
  - 스프링 빈 등록 : 전체 다 등록
  - 의존 관계 주입 : `@Autowired ` 있는 경우 자동 주입
  - `@Autowired`
    - **타입 기준**으로 의존 관계 주입
    - **스프링 컨테이너에 주입할 스프링 빈이 없으면 오류 발생**
    - `@Autowired(required = false)`
      - 주입할 대상이 없어도 실행 가능
- 방법
  - 생성자 주입

    - 생성자 호출 시점에 **딱 한 번만 호출**
    - **스프링 빈 등록 후 바로 의존 관계 주입**(처음에 객체 생성시 생성자가 필요하니까)
    - **불변, 필수**
      - 불변 : 처음에 한 번 의존 관계 주입 후 수정X
      - 필수 : final(초기화 안하면 컴파일 에러 발생)
    - **생성자 한 개 인경우 `@Autowired` 생략 가능**

  - 수정자 주입

    - 선택, 변경
      - 선택 : 특정 필드만 선택해서 의존관계 주입
      - 변경 : 중간에 의존 관계 변경할 때
    - 참고) 생성자 주입과 수정자 주입이 모두 있다면?
      - 생성자 주입 후 수정자 주입 발생

    - 참고) 자바빈 프로퍼티 규약
      - setXXX, getXXX 쓰는 것 : 메서드를 통해 값을 얻거나 변경

  - 필드 주입

    - 의존 관계를 필드에 바로 주입
    - 사용하지 말자
      - 스프링 컨테이너에서만 의존 관계 주입 가능 -> 스프링 컨테이너를 사용하지 않는 외부에서 사용 불가 (ex) 테스트 -  `@SpringBootTest`는 예외 )
      - 즉, 순수 자바 코드로 의존 관계 주입할 방법이 없음

    - 사용하는 경우
      - `@SpringBootTest`
      - `@Configuration`

  - 일반 메서드 주입
    - 한 번에 여러 필드 주입 가능
    - **잘 안쓴다**

<br>

#### 옵션 처리

주입할 스프링 빈이 없을 때도 동작해야하는 경우

- 방법
  - `@Autowired(required = false)` : 자동 주입할 대상이 없다면 호출 자체가 안됨
  - `@Nullable` : 자동 주입할 대상이 없으면 null
  - `@Optional<T>` : 자동 주입할 대상이 없으면 `Optional.empty` 입력

<br>

#### 생성자 주입을 선택해라

- 이유

  - 불변 
    - 대부분의 경우 의존 관계 주입이 애플리케이션 종료 시점까지 변경할 일이 없음
    - 생성자 주입은 객체 생성할 때 **딱 한 번만 호출** 되므로 불변하게 설계 가능
  - 누락
    - 수정자 주입시 의존 관계 누락 가능 -> 직관적으로 어떤 의존 관계가 필요한지 파악X
      - 순수한 자바 코드만으로 테스트 시 누락 발생 가능성 증가
    - 생성자 주입의 경우 의존관계 누락시 컴파일 오류 발생

  - final 키워드
    - 멤버 변수 초기화 누락 문제 해결
    - 초기화시 반드시 의존 관계 주입
  - 참고) 컴파일 오류가 세상에서 가장 빠르고 좋은 오류

- 정리

  - 생성자 주입 써라
  - 종종 옵션 필요할 때 수정자 주입 쓰자
  - 필드 주입은 쓰지 말자

<br>

#### 롬복과 최신 트렌드

- 롬복 
  - 어노테이션 기반으로 편리한 기능 제공(생성자, getter, setter, toString, ....)

- 롬복 연동하기
  - `build.gradle`에 의존성 추가
  - lombok 다운
  - `Annotation Processors` 에서 `Enable annotation processing` 체크
- `@RequiredArgsConstructor`
  - 현재 객체의 final이 붙은 멤버 변수들을 초기화 해주는 생성자 자동 생성
  - 생성자가 하나인 경우 `@Autowired`가 생략 가능해서 사용 가능
- 참고) 인텔리제이 단축키
  -  `ctrl + F12` : 현재 클래스의 구성 요소 확인(필드, 메서드, 생성자, ..)

<br>

#### 조회 빈이 2개 이상 - 문제

- 문제 원인 
  - `@Autowired` 는 기본적으로 **타입**으로 조회 
  - `ac.getBean(XXX.class)` 와 비슷
- 결과 : `NoUniqueBeanDefinitionException` 발생 

<br>

#### `@Autowired` 필드 명, `@Qualifier`, `@Primary`

- 조회 대상 빈이 2개 이상일 떄 해결
  - `@Autowired` 필드명 매칭
  - `@Qualifier`
  - `@Primary`

- `@Autowired` 필드명 매칭
  - 먼저, 타입 매칭 시도
  - 여러 빈이 있다면, **필드 이름, 파라미터 이름으로 빈 이름 추가 매칭**

- `@Qualifier`

  - 추가 구분자 : 주입시 추가적인 방법 제공 (빈 이름 변경X)

  - `@Qualifier` 끼리만 매칭

    - 빈 등록시 :  `@Qualifier("이름")`
    - 주입 시 : `@Qualifier("이름")`

  - 추가 구분자인 스프링 빈을 못찾을 때

    - 추가 구분자를 이름으로 가지는 스프링 빈 찾음

    - 그래도 없으면 예외 처리

- `@Primary`

  - 우선 순위 지정 : `@Primary` 가 있는 빈이 주입

- @Qualifier와 @Primary

  - 활용
    - 메인 DB와 서브 DB가 있을 떄
      - 메인 : `@Primary`
      - 서브 : `@Qualifier`
      - 둘다 `@Qualifier` 로 쓸 수도 있다
  - 우선 순위
    - `@Qualifier` 가 높다(수동이 항상 더 높다)

<br>

#### 애노테이션 직접 만들기

- `@Qualifier("xxx")`의 문제점
  - 내부가 문자열이라 컴파일시 잘못 주입했는지 체크가 안됨

- 애노테이션을 직접 만들어 컴파일시 체크 가능
  - 내부적으로 `@Qualifier` 사용
- `@Primary` 로 해결 가능하다면 `@Primary` 쓰자
- 참고) 애노테이션은 상속 기능이 없다, 스프링에서 지원해주는 것임

<br>

#### 조회한 빈이 모두 필요할 때 List, Map

- 의도적으로 해당 타입의 스프링 빈이 다 필요한 경우 존재

  - ex) 클라이언트가 정액할인, 정률할인 중 택일 할 때

- 적용

  - `Map<String, T>`

    - 타입이 T인 모든 스프링 빈이 주입된다
    - key : 빈 이름, value : 스프링 빈 객체

  - `List<T>`

    - 타입이 T인 모든 스프링 빈이 주입된다

  - 코드

    ```java
    static class DiscountService {
            private final Map<String, DiscountPolicy> policyMap;
            private final List<DiscountPolicy> policies;
    
            public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
                this.policyMap = policyMap;
                this.policies = policies;
                System.out.println("policyMap = " + policyMap);
                System.out.println("policies = " + policies);
            }
    
            public int discount(Member member, int price, String discountCode) {
                DiscountPolicy discountPolicy = policyMap.get(discountCode);
                return discountPolicy.discount(member, price);
            }
    }
    ```

- 개인적으로 빈 이름으로 스프링 빈 객체에 접근할 수 있는 Map을 사용하는 것이 좋은 것 같다 

<br>

#### 자동, 수동의 올바른 실무 운영 기준

- 애플리케이션에서 사용하는 스프링 빈

  - 업무 로직 빈 - **자동**
    - 핵심 비즈니스 로직 (컨트롤러, 서비스, 리파지토리, ....)
    - 양이 많다
  - 기술 지원 빈 - **수동**
    - 기술적인 문제 or AOP 처리시 (로깅, DB 연결, ...)
    - 상대적으로 양이 적다
  - 예외 
    - 비즈니스 로직에서 다형성을 적극 활용할 때
      - 한 눈에 딱 보고 파악 가능하게 함
        - 수동 사용 - 한눈에 설정 정보를 파악 가능
        - 자동 사용 - **패키지에 한번에 묶어 두는 방법**
    - **자동 설정**

<br>

### 빈 생명 주기 콜백

#### 빈 생명 주기 콜백 시작

스프링에서 객체의 초기화와 종료가 어떻게 진행되는지 알아보자

- 스프링 빈의 라이프사이클(간단버전)
  - 객체 생성
  - 의존 관계 주입
  - cf) 생성자주입은 객체 생성 후 동시에 의존관계 주입
- 스프링 빈의 이벤트 라이프사이클(싱글톤 기준)
  - 스프링 컨테이너 생성
  - 스프링 빈 생성
  - 의존 관계 주입
  - **초기화 콜백 : 빈의 의존관계 주입까지 완료 후 호출** 
  - 사용 : 스프링 빈을 비즈니스 로직에서 사용하기 전에 초기화까지 완료
  - **소멸전 콜백 : 빈이 소멸되기 직전에 호출**
  - 스프링 종료
- **객체의 생성과 초기화를 분리**하자 -> 유지보수 
  - 생성자
    - 필수 파라미터를 받고
    - 메모리를 할당받아서 객체를 생성하는 역할
  - 초기화
    - 생성자를 통해 생성된 값들을 통해 실제 동작 수행
    - 객체를 생성하는 것 보다 무거운 작업(ex)외부와 연동) 
- 싱글톤 빈의 소멸
  - 스프링 컨테이너가 종료( `close()` ) 되면 같이 소멸

<br>

#### 인터페이스 InitializingBean, DisposableBean

- InitialzingBean
  - 초기화 진행
  - `afterPropertiesSet()`

- DisposableBean

  - 소멸 진행
  - `destroy()`

- 단점

  - 스프링 전용 인터페이스 (org.springframework)
  - 메서드명 변경 불가 (오버라이딩)
  - **외부 라이브러리에 적용 불가** (내부 코드를 수정해야하므로)

  - **거의 사용안함**

<br>

#### 빈 등록 초기화, 소멸 메서드

- 사용법
  - `@Bean(initMethod = "초기화 메소드", destroyMethod = "소멸 메서드")`
- 특징
  - 메서드명 자유롭게 작성
  - 스프링 코드에 의존X
  - **외부 라이브러리에도 적용가능**
- 종료(소멸) 메서드
  - `destroyMethod` 의 기본값 : `"{infer}"`
    - 추론 기능 : `close`나 `shutdown` 이라는 이름의 메소드 자동 호출
    - 외부라이브러리 대부분은 종료 메서드가 `close`나 `shutdown` 임

<br>

#### 애노테이션 @PostConstruct, @PreDestroy

이거 써라

- @PostConstruct : 초기화 메서드
- @PreDestroy : 종료 메서드
- 특징
  - 편리
  - 스프링 컨테이너에 의존X ->  순수 자바 `javax`
  - 컴포넌트 스캔과 어울림
  - **외부라이브러리에 적용불가** (내부 코드를 수정해야하므로)

- 정리
  - `@PostConstruct, @PreDestroy` 사용
  - 외부라이브러리 초기화 및 종료가 필요하면 `@Bean(initMethod = "초기화 메소드", destroyMethod = "소멸 메서드")` 사용

<br>

### 빈 스코프

#### 빈 스코프란?

- 스프링에서 지원하는 다양한 스코프

  - 싱글톤

    - 기본 스코프
    - 스프링 컨테이너의 시작과 종료까지 유지

  - 프로토타입

    - 스프링 컨테이너가 빈의 생성과 초기화 까지만 관여

    - 이후에는 컨테이너가 관리 안함

  - 웹 관련 스코프

    - `request` : 요청이 들어오고 나갈 때 까지 유지
    - `session` : 세션이 생성되고 종료될 때 까지 유지
    - `application` : 웹의 서블릿 컨텍스트와 같은 범위로 유지

- 빈 스코프 등록

  - `@Scope`

<br>

#### 프로토타입 스코프

- 프로토타입 스코프란
  - 프로토타입 빈 요청(조회)시 매번 다른 객체 생성 후 의존관계 주입
  - 클라이언트에게 반환 후 컨테이너에서 더 이상 관리X
  - 정리
    - **스프링 컨테이너는 프로토타입 빈을 생성, 의존관계 주입, 초기화 까지만 지원**
    - **`@PreDestroy` 불가능**

- 싱글톤 스코프 vs 프로토타입 스코프
  - 싱글톤
    - 스프링 컨테이너 생성 시점에 초기화 메서드 실행
    - 초기화 후에도 스프링 컨테이너가 관리 -> 컨테이너 종료시 종료 메서드 실행
  - 프로토타입
    - 스프링 컨테이너에서 빈 조회시 객체가 새로 생성
    - 스프링 컨테이너가 객체의 생성, 의존 관계 주입, 초기화 까지만 지원
    - 초기화 후 스프링 컨테이너가 관리X 
      - 컨테이너 종료 후 종료 메서드 실행X
      - 클라이언트가 직접 관리

<br>

#### 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 문제점

- 싱글톤 빈이 프로토타입 빈에 의존할 때

  - 싱글톤 빈은 스프링 컨테이너 생성 시점에 함께 생성

  - 싱글톤 빈 생성후 의존관계 주입, 이떄 스프링 컨테이너가 프로토타입 빈 생성하여 반환

  - **싱글톤 빈은 생성 시점에 단 한 번 의존관계를 주입 받음**

    - **사용(조회) 할 때마다 프로로타입 빈이 새로 생성되지 않음**

    - **사용할 떄 마다 하나의 프로토타입 빈이 계속 반환**

- 싱글톤 빈이 프로토타입 빈을 사용할 떄마다 새로 생성되게 하려면 어떻게 해야하나???

  - 무식한 방법 : ac에서 프로토타입 빈 조회하여 프로토타입 빈 새로 생성

<br>

#### 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 Provider로 문제 해결

- Dependency Lookup(DL)

  - 의존관계 조회
    - 외부에서 의존 관계를 주입 받는 것이 아니라, 직접 필요한 의존 관계를 조회해서 맺는것
  - ex) ac에서 프로토타입 빈을 직접 조회하는 것

-  ObjectFactory, ObjectProvider

  - DL을 대신 해줌
    - 메소드 : `getObject()`
    - 스프링에 의존적
  - ObjectProvider가 ObjectFactory를 상속한 것 -> 지원 기능이 더 많음

- JSR-330 Provider

  - 자바 표준 (`javax.inject`) : 스프링이 아닌 다른 컨테이너에서 사용 가능
  - `javax.inject:javax.inject:1` 라이브러리 추가

  - 메소드 : `get()`

- 프로토타입 빈은 언제 사용하는가?

  - 매번 사용할 떄 마다 의존관계 주입이 완료된 새로운 객체가 필요한 경우

  - 실무에서 거의 쓸 일 없음 : 객체가 새로 필요한 경우 `new`로 매번 생성해서 씀

<br>

#### 웹 스코프

- 웹 스코프란
  - 웹 환경에서만 동작
  - 스프링이 종료 시점까지 관리
- 종류
  - request : HTTP 요청이 들어오고 응답할 때 까지 유지 (요청이 들어올 떄 생성, 응답 후 소멸)
  - session : HTTP session과 동일한 생명주기 가짐
  - application : 서블릿 컨텍스트와 동일한 생명주기 가짐
  - websocket : 웹 소켓과 동일한 생명주기 가짐

<br>

#### request 스코프 예제 만들기

- 환경 추가
  - 웹 라이브러리 추가
  - `implementation 'org.springframework.boot:spring-boot-starter-web'`

- request 예제 개발

  - 상황
    - 동시에 여러 HTTP 요청 발생

  - `UUID` : 고유한 request 번호

  - 클래스

    - MyLogger
      - 변수
        - `uuid` 
        - `requestURL`
      - 메소드
        - `log` : 로그 찍기
    - LogDemoController
      - 변수
        - LogDemoService
        - MyLogger
      - 메소드
        - logDemo : 로그 찍기
    - LogDemoService
      - 변수
        - LogDemoService
        - MyLogger
      - 메소드
        - logic : 로그 찍기

  - 오류 발생

    - `...Scope 'request' is not active....`

    - 원인
      - MyLogger는 request 스코프
      - HTTP 요청이 들어와야 빈이 생성
      - 컨트롤러와 서비스에서 생성자 의존 관계 주입 불가

  - 참고

    - 로그는 스프링 인터셉터나 서블릿 필터로 하는게 더 간편하다

  - request 스코프의 장점

    - 코드 중복을 줄일 수 있음 -> 파라미터 최소화
    - 서비스가 웹 기술에 종속 되지 않게함 
      - 예제에서 requestURL과 같은 웹 관련 정보가 서비스 계층에 전달X
      - 웹 기술은 컨트롤러 까지만 구현하는게 좋음
      - 서비스는 순수하게 유지하는 것이 좋음
      - 유지보수 향상

<br>

#### 스코프와 Provider

- Provider로 해결

  - ObjectProvider활용
    - `ObjectProvider`를 통해 request scope 빈의 생성 지연가능
    - HTTP 요청이 진행중일 때 `getObject()` 를 호출하므로 빈 생성 정상적으로 진행
    - 계층이 달라도 같은 HTTP 요청이면 같은 request scope 호출

- 결과

  ```
  [09d82dd9-3dfb-4879-8dd8-513c255403d7] request scope bean create : hello.core.common.MyLogger@25afb738  --> 요청이 들어오면 생성 및 초기화
  [09d82dd9-3dfb-4879-8dd8-513c255403d7][http://localhost:8080/log-demo] controller test --> 컨트롤러 로그
  [09d82dd9-3dfb-4879-8dd8-513c255403d7][http://localhost:8080/log-demo] service id = testId --> 서비스 로그
  [09d82dd9-3dfb-4879-8dd8-513c255403d7] request scope bean close : hello.core.common.MyLogger@25afb738 --> 응답 후 소멸
  ```

- **동시에 여러 요청이 들어올떄 각 요청에 대해 빈을 생성하여 하나씩 처리 가능**

<br>

#### 스코프와 프록시

- proxyMode 설정

  - 방법
    - 클래스
    - 인터페이스
  - 가짜 프록시 객체 주입
    - CGLIB을 통해 클래스를 상속 받은 가짜 프록시 객체 생성 (다형성)
    - 프록시 객체는 내부에 실제 객체를 찾아오는 법을 알고 있음
    - 객체의 메소드가 실행되면 스프링 컨테이너를 통해 실제 객체의 메소드를 호출한다

- 정리

  - **Provider를 쓰던 프록시를 사용하던 진짜 객체 조회를 필요한 시점까지 지연처리 하는 것이 핵심**

- 참고

  - 프록시는 스프링 AOP에서도 활용된다

<br>
