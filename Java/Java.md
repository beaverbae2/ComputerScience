### Java

- [객체 지향 프로그래밍](#객체-지향-프로그래밍)
- [객체 지향의 4가지 특징](#객체-지향의-4가지-특징)

- [final/static/abstract](#final/static/abstract)

- [추상클래스/인터페이스](#추상클래스/인터페이스)

<br>

#### 객체 지향 프로그래밍

- 컴퓨터 프로그래밍을 객체와 객체간의 상호작용으로 바라봄
- 구성 요소
  - 클래스 (현실 세계의 물체를 바라보듯이)
    - 속성: 변수(멤버변수)
    - 행위: 메소드
  - 객체
    - 클래스의 인스턴스
    - 인스턴스 : 클래스에서 정의한 것을 실제 메모리에 할당
- 특징
  - strong cohesion : 한 모듈내에 존재하는 변수, 함수 사이의 밀접한 정도가 강해야함 (클래스가 담당한 기능만 확실하게 구현)
  - loose coupling : 서로 다른 모듈 간의 의존성은 느슨해야함 (의존성이 높아지면 수정시 고쳐야하는게 많음)

<br>

#### 객체 지향의 4가지 특징

- 추상화

  - 여러 객체에 **공통적으로 사용**되는, 사용되어야 하는 내용(변수, 메소드)을 뽑아내는 것

  - 공통된 내용을 부모 클래스로 뽑아냄

- 캡슐화

  - 클래스에서 중요한 변수를 보존, 보호, 메소드를 통해 변수에 접근
  - 사용법
    - 멤버 변수의 접근 제어자 private
    - getter(변수 획득), setter(변수 변경) 설정
  - 접근 제어자
    - private : 변수를 선언한 클래스에서만 접근 가능
    - (default) : 같은 패키지에 있는 클래스에서만 접근 가능
    - protected : 같은 패키지 또는 상속받은 클래스에서 접근 가능
    - public :  모든 클래스에서 접근 가능

- 상속

  - 클래스의 멤버 변수와 메소드를 다른 클래스에게 물려주는 것
  - **부모 객체 생성 후 자식 객체 생성**
  - super() : 조상 클래스의 생성자 호출
  - super : 조상 객체
  - 참고
    - this() : 같은 클래스내의 다른 생성자 호출
    - this : 클래스 자신
  - 장점
    - 코드의 재사용성이 증가
    - 확장성이 증가

- 다형성

  - **조상클래스 타입의 참조변수로 자손클래스의 인스턴스 참조**
  - 같은 타입의 인스턴스여도 참조 변수에 따라 사용할 수 있는 멤버가 다름
  - 형 변환 : 자식 타입 <- 부모 타입
  - **오버라이딩**
    - 다형성의 핵심
    - 자식 클래스에 부모 클래스의 메소드를 **재정의** 
    - 참조 변수가 아닌 **인스턴스를 따라간다**
  - 장점 : 매개변수 다형성

- 예제 코드

  ```java
  class Parent {
      private int a;// 접근 제어
      private int b;// 접근 제어
      
      public Parent(){
          super();// Object클래스 생성
      }
      
      public void method(){
          // logic
      }
      
      //getter, setter....
  }
  
  class Child extends Parent {
      private int c;// 접근 제어
      
      public Child(){
          super();// Parent클래스 생성
      }
      
      @Override
      public void method(){
          // logic
      }
      
      //getter, setter....
  }
  
  public class Main {
      public static void main(String[] args){
          Parent p1 = new Parent();
          Parent p2 = new Child();// 멤버 변수는 a, b만 사용가능, 오버라이딩 된 자식 메소드 호출
          Child c1 = new Child();
          Child c2 = (Child) new Parent();// 형변환, 참조 변수를 따른다(멤버변수)
      }
  }
  ```

<br>

### final/static/abstract

- final
  - 클래스 : 상속 불가
  - 메소드 : 오버라이딩 불가
  - 변수 : 수정 불가(상수)
- static
  - 클래스 (참고용)
    - static inner 클래스 : 부모 인스턴스 생성과는 상관없이 사용
    - Nonstatic inner클래스 : 부모 인스턴스 생성 후 사용가능
  - 메소드 : 멤버 변수 접근 불가 (클래스 메소드)
  - 변수 : 모든 객체에서 공유 (클래스 변수)
  - ex) Math.min(4, 19)
- abstract
  - 클래스 : 추상 클래스 - 클래스 내에 추상 메소드 존재
  - 메소도 : 추상 메소드 - 구현부가 없음

<br>

#### 추상클래스/인터페이스

**추상클래스**

- 추상클래스

  - 추상화(여러 객체간 공통된 내용 추출)를 통해 만들어진 클래스
  - 자식 클래스에서 상속 받아 확장하는 것이 목적

  - 추상메소드가 반드시 **한 개 이상 존재**

- 추상메소드

  - 구현부가 없는 메소드
  - **반드시 자식 클래스에서 오버라이딩해서 구현**해야한다

**인터페이스**

- 인터페이스

  - **모든 메소드가 추상메소드**
    - abstact 키워드 생략

  - 멤버변수는 final만 존재 가능
  - 명세서 같은 느낌(상속받아서 이러한 기능을 구현해 주세요)

<br>

**추상클래스 vs 인터페이스**

- 상속
  - 추상클래스 : extends
  - 인터페이스 : implements
- 인터페이스만 다중 상속 가능