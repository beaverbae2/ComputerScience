##  Java

- [JVM](#JVM)
- [GC의 동작 원리](#GC의-동작-원리)

- [객체 지향 프로그래밍](#객체-지향-프로그래밍)
- [객체 지향의 4가지 특징](#객체-지향의-4가지-특징)
- [final static abstract](#final-static-abstract)
- [추상클래스와 인터페이스](#추상클래스와-인터페이스)

<br>

### JVM

**참고 자료**

- [JVM](https://asfirstalways.tistory.com/158)
- [GC](https://asfirstalways.tistory.com/159)
- [GC의 과정](https://d2.naver.com/helloworld/1329)

#### 자바 파일의 실행 과정

- 프로그램 실행
- JVM이 운영체제로 부터 메모리 할당
- javac(자바 컴파일러)가 .java -> .class(byte code)로 변환
- Class Loader가 .class 파일 받아서 Runtime Data Area로 전달
- byte code는 Runtime Data Area에 배치되어 실행, 필요에 따라 JVM이 쓰레드 동기화와 GC작업 수행
- Execution Engine을 통해 (기계어로 번역되어) 실행

#### JVM(Java Virtual Machine)의 구조

- Class Loader
  - .class(byte code) 파일을 **동적로딩**하여 Runtime Data Area(Method Area)로 보냄
  - 동적 로딩 : 모든 클래스를 다 로딩하는게 아니고 필요한 클래스만 로딩
- Runtime Data Area
  - JVM이 운영체제로 부터 할당 받은 메모리 영역
  - 모든 쓰레드가 공유하는 영역
    - Method Area
      - JVM 시작시 실행
      - byte code 저장
      - 클래스에 대한 정보 저장
        - field :  멤버 변수명, 타입, 접근제어자
        - method : 메소드 명, 리턴 타입, 접근제어자
        - type : 클래스인지 인터페이스 인지
      - runtime contraint pool
        - 클래스 구성 요소들의 주소 저장
        - 메소드나 필드 주소를 이곳에서 참조
    - Heap Area
      - new 로 생산된 객체를 저장하는 영역, GC의 대상
  - 각 쓰레드 별 생성
    - Stack Area
      - 메소드 호출시 마다 생성(push)
      - 메소드의 구성요소(지역변수, 파리마터, 리턴값) 저장
      - 메소드 종료 후 삭제(pop)
    - PC register
      - 쓰레드 시작시 발생
      - 현재 수행중인 JVM의 주소
    - Native Method Stack
      - 자바 외의 언어(C, C++)로 작성된 네이티브 코드(기계어)를 위한 영역
  
- Execution Engine
  - .class(byte code) 파일을 기계어(native code)로 번역(compile)후 명령어 실행
  - 명령어 실행 방법
    - Interpreter : 하나씩 실행
    - JIT : 특정 시간에 전체 byte code를 native code로 변환
    - compile 비교
      - javac : source code -> byte code
      - interpreter : byte code -> native code 
  - Garbage Collector
    - heap에 있는 객체 중 참조 되지 않는 객체 삭제

<br>

### GC의 동작 원리

참고 자료

- [우아한Tech - Garbage Collector](https://www.youtube.com/watch?v=vZRmCbl871I)
- [Naver D2 - Java Garbage Collection](https://d2.naver.com/helloworld/1329)
- [(JVM) Garbage Collection Advanced](https://perfectacle.github.io/2019/05/11/jvm-gc-advanced/)

#### Heap의 구조

- Young(New) Generation 영역 : Eden, Survivor0, Survivor1
- Old Generation 영역 : Old

#### 과정

**Stop the world**

- GC 발생시, GC 스레드를 제외한 모든 스레드가 멈추는 것
- **GC의 성능은 stop the world를 얼만큼 줄일 수 있느냐에 비례**

**Mark and Sweap**

- Mark
  - GC가 **Stack의 모든 변수를 스캔**하며 각각 어떤 객체를 참조하고 있는지 찾아서 마킹
  - 즉, **Reachable한 객체를 찾아서 마킹**
- Sweap
  - **Mark되지 않은 객체를 Heap에서 삭제**
- GC가 발생한다 == Mark and Sweap

#### Minor GC

- Young 영역(Eden, Survivor0, Survivor1)에서 발생 
- 동작과정
  - 요약
    - Eden : 생성된 객체가 쌓이는 곳, 이곳이 꽉차면 GC 발생
    - Survivor : GC 발생 후 남은 객체들이 쌓이는 곳
      - **Survivor0, Surivivor1 둘 중 하나만 쓰인다**
  - 구체적인 과정 
    - 새로 생성한  Eden에 쌓임
    - Eden 영역에서 GC 발생, 살아 남은 객체는 Survivor영역중 하나로 이동
    - 살아남은 객체들이 Survivor에 지속적으로 쌓임
    - 하나의 Survivor 영역이 가득차게 되면 GC 발생, 살아남은 객체는 다른 Survivor 영역으로 이동(**age 증가**)
    - 이 과정을 지속적으로 반복하다 계속 살아 있는 객체들은 Old 영역으로 이동 

#### Major GC(Full GC)

- Old 영역에서 발생
- 종류
  - Serial GC
    - GC를 처리하는 쓰레드가 한 개, 시간이 오래 걸림
    - Mark-Sweap-Compact 
      - Mark and Sweap 하는건 동일
      - Compact : 파편화된 할당 메모리 한쪽으로 모음(살아있는 객체들이 메모리에 연속되게 존재하도록 함)
    
  - Parallel GC
    - Serial GC와 동일, 단 GC 처리 쓰레드가 여러개
    - java8 default GC
    
  - CMS(Concurrent Mark Sweap) GC 
    - 과정

      - Initial mark :  **stack에서 참조**하는 reachable한 객체 마킹 (stop the world)

      - Concurrent mark : intial mark때 확인했던 **reachable한 객체들이 참조하는 객체들을 따라가며 마킹** (stop the world X (다른 쓰레드들도 진행 중))
      - Remark : Concurrent mark 단계에서 새로 추가되거나 참조가 끊긴 객체 확인 -> 최종적으로 reachable한 객체 확인 (stop the world)
      - Concurrent Sweap :  객체 제거 (stop the world X)

    - 특징
      - Stop-the-World 기간을 줄임 -> 빠름
      - Compact알고리즘 진행X

  - G1 GC
    
    - 기존 GC 원리와 다름
    - heap 영역을 고정된 크기의 영역으로 나눔
    - 이 영역에 객체를 할당
    - 영역이 꽉차면 다른 빈 영역에서 객체 할당 하게 되고, **해당 영역에 대해서만 GC 실행**
    - 특징
      - Stop-the-World 기간을 줄임 -> 빠름
      - Compact알고리즘 진행

<br>

### 객체 지향 프로그래밍

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

### 객체 지향의 4가지 특징

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

### final static abstract

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
  - 클래스 : 추상 클래스 - 클래스 내에 추상 메소드 존재가능
  - 메소드 : 추상 메소드 - 구현부가 없음 -> 자식클래스에서 오버라이딩

<br>

### 추상클래스와 인터페이스

**추상클래스**

- 추상클래스

  - 추상화(여러 객체간 공통된 내용 추출)를 통해 만들어진 클래스
  - 자식 클래스에서 상속 받아 확장하는 것이 목적
- 추상메소드가 존재 가능
  - 미완성 설계도

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