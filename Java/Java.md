##  Java

#### 차례

- [JVM](#JVM)
- [GC의 동작 원리](#GC의-동작-원리)
- [객체 지향 프로그래밍](#객체-지향-프로그래밍)
- [객체 지향의 4가지 특징](#객체-지향의-4가지-특징)
- [final static abstract](#final-static-abstract)
- [추상클래스와 인터페이스](#추상클래스와-인터페이스)
- [제네릭](#제네릭)
- [불변 객체](#불변-객체)
- [예외처리](#예외처리)
- [문자열 처리](#문자열-처리)
- [직렬화](#직렬화)
- [Comparable과 Comparator](#Comparable과-Comparator)
- [익명 클래스](#익명-클래스)
- [람다식](#람다식)

#### 별첨

- [객체 지향 보충자료](./객체_지향_보충자료.md)

<br>

### JVM

**참고 자료**

- [Jbee - JVM](https://asfirstalways.tistory.com/158)
- [Naver D2 - JVM](https://d2.naver.com/helloworld/1230)

#### JVM

- Java Virtual Machine
- 자바 프로그램과 OS사이에서 중개자 역할
  - OS에 구애받지 않고 실행 가능
- 자바 프로그램을 실행해주는 역할
  - 메모리 관리 
  - GC

#### 자바 프로그램의 실행 과정

- 프로그램 실행
- JVM이 OS로 부터 메모리 할당
- javac(자바 컴파일러)가 .java -> .class(byte code)로 변환
- Class Loader를 통해 JVM에 class파일 로딩
- Class Loader가 byte code를 Runtime Data Area에 배치
- 배치된 byte code는 Execution Engine에 의해 실질적으로 수행
  - 이떄 필요에 따라 JVM은 쓰레드 동기화나 GC 진행

#### JVM(Java Virtual Machine)의 구조

- Class Loader
  - .class(byte code) 파일을 **동적로딩**하여 Runtime Data Area로 보냄
  - 동적 로딩 
    - 모든 클래스를 다 로딩하는게 아니고 **런타임시 클래스를 처음 참조할 때 로딩**
    - 로딩 : 메모리(Runtime Data Area)에 적재
  - 동적 링킹
    - 링킹(Linking) : 프로그램 실행에 필요한 라이브러리나 다른 프로그램을 합침 (import)
      - 정적 링킹 : **컴파일 시에** 프로그램 실행에 필요한 모든 라이브러리 모듈 복사 
      - 동적 링킹 : 모든 라이브러리 모듈 복사X, 주소만 가지고 있다가 **런타임 시에 필요한 라이브러리 모듈 복사**
    - 검증(verifying) : 클래스가 자바 언어 명세대로 잘 구성되어 있는지 검사
    - 준비(preparing) : 클래스가 필요로 하는 메모리 할당, 클래스에 정의된 필드, 메소드, 인터페이스에 대한 데이터 구조 준비
    - 분석(Resolving) : 클래스 상수 풀 내의 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경
  - 초기화 
    - 클래스 변수 초기화

- Runtime Data Area (JVM Memory)

  - JVM이 운영체제로 부터 할당 받은 메모리 영역

  - Class Loader로 부터 byte code가 전달된다

  - 구조

    - 모든 쓰레드가 공유하는 영역
      - Method Area
        - **클래스 정보를 처음 메모리로 올릴때 초기화되는 대상을 저장하는 메모리 공간**

        - 저장되는 클래스 정보
          
          - field :  멤버 변수명, 타입, 접근제어자
          - method : 메소드 명, 리턴 타입, 접근제어자
          - type : 클래스인지 인터페이스 인지
          
        - **runtime contraint pool (런타임 상수 풀)**

          - 다음의 정보가 심볼릭 레퍼런스(논리적 주소) 형식으로 저장된다

            - 클래스또는 인터페이스의 상수(리터럴)가 저장
            - **클래스, 메소드, 필드의 주소 저장**

          - 실행 예제 (궁금해서 실제로 해봤음당)

            - 소스코드

              ```java
              public class Test {
              	public static void main(String[] args) {
              		Pair p1 = new Pair();
              		p1.setA(2);
              		p1.setB(3);
              		System.out.println(p1.a);
              		
              		Pair p2 = new Pair();
              		p2.setA(2);
              		p2.setB(3);
              		System.out.println(p2.a);
              	}
              }
              
              class Pair {
              	int a, b;
              
              	public void setA(int a) {
              		this.a = a;
              	}
              
              	public void setB(int b) {
              		this.b = b;
              	}
              }
              
              ```

            - 바이트코드(javap 사용)

              - constraint pool

                ```java
                Constant pool:
                   #1 = Class              #2             // bytecode/Test
                   #2 = Utf8               bytecode/Test
                   #3 = Class              #4             // java/lang/Object
                   #4 = Utf8               java/lang/Object
                   #5 = Utf8               <init>
                   #6 = Utf8               ()V
                   #7 = Utf8               Code
                   #8 = Methodref          #3.#9          // java/lang/Object."<init>":()V
                   #9 = NameAndType        #5:#6          // "<init>":()V
                  #10 = Utf8               LineNumberTable
                  #11 = Utf8               LocalVariableTable
                  #12 = Utf8               this
                  #13 = Utf8               Lbytecode/Test;
                  #14 = Utf8               main
                  #15 = Utf8               ([Ljava/lang/String;)V
                  #16 = Class              #17            // bytecode/Pair
                  #17 = Utf8               bytecode/Pair
                  #18 = Methodref          #16.#9         // bytecode/Pair."<init>":()V
                  #19 = Methodref          #16.#20        // bytecode/Pair.setA:(I)V
                  #20 = NameAndType        #21:#22        // setA:(I)V
                  #21 = Utf8               setA
                  #22 = Utf8               (I)V
                  #23 = Methodref          #16.#24        // bytecode/Pair.setB:(I)V
                  #24 = NameAndType        #25:#22        // setB:(I)V
                  #25 = Utf8               setB
                  #26 = Fieldref           #27.#29        // java/lang/System.out:Ljava/io/PrintStream;
                  #27 = Class              #28            // java/lang/System
                  #28 = Utf8               java/lang/System
                  #29 = NameAndType        #30:#31        // out:Ljava/io/PrintStream;
                  #30 = Utf8               out
                  #31 = Utf8               Ljava/io/PrintStream;
                  #32 = Fieldref           #16.#33        // bytecode/Pair.a:I
                  #33 = NameAndType        #34:#35        // a:I
                  #34 = Utf8               a
                  #35 = Utf8               I
                  #36 = Methodref          #37.#39        // java/io/PrintStream.println:(I)V
                  #37 = Class              #38            // java/io/PrintStream
                  #38 = Utf8               java/io/PrintStream
                  #39 = NameAndType        #40:#22        // println:(I)V
                  #40 = Utf8               println
                  #41 = Utf8               args
                  #42 = Utf8               [Ljava/lang/String;
                  #43 = Utf8               p1
                  #44 = Utf8               Lbytecode/Pair;
                  #45 = Utf8               p2
                  #46 = Utf8               SourceFile
                  #47 = Utf8               Test.java
                ```

              - 바이트 코드

                ```java
                {
                  public bytecode.Test();
                    descriptor: ()V
                    flags: (0x0001) ACC_PUBLIC
                    Code:
                      stack=1, locals=1, args_size=1
                         0: aload_0
                         1: invokespecial #8                  // Method java/lang/Object."<init>":()V
                         4: return
                      LineNumberTable:
                        line 3: 0
                      LocalVariableTable:
                        Start  Length  Slot  Name   Signature
                            0       5     0  this   Lbytecode/Test;
                
                  public static void main(java.lang.String[]);
                    descriptor: ([Ljava/lang/String;)V
                    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
                    Code:
                      stack=2, locals=3, args_size=1
                         0: new           #16                 // class bytecode/Pair
                         3: dup
                         4: invokespecial #18                 // Method bytecode/Pair."<init>":()V
                         7: astore_1
                         8: aload_1
                         9: iconst_2
                        10: invokevirtual #19                 // Method bytecode/Pair.setA:(I)V
                        13: aload_1
                        14: iconst_3
                        15: invokevirtual #23                 // Method bytecode/Pair.setB:(I)V
                        18: getstatic     #26                 // Field java/lang/System.out:Ljava/io/PrintStream;
                        21: aload_1
                        22: getfield      #32                 // Field bytecode/Pair.a:I
                        25: invokevirtual #36                 // Method java/io/PrintStream.println:(I)V
                        28: new           #16                 // class bytecode/Pair
                        31: dup
                        32: invokespecial #18                 // Method bytecode/Pair."<init>":()V
                        35: astore_2
                        36: aload_2
                        37: iconst_2
                        38: invokevirtual #19                 // Method bytecode/Pair.setA:(I)V
                        41: aload_2
                        42: iconst_3
                        43: invokevirtual #23                 // Method bytecode/Pair.setB:(I)V
                        46: getstatic     #26                 // Field java/lang/System.out:Ljava/io/PrintStream;
                        49: aload_2
                        50: getfield      #32                 // Field bytecode/Pair.a:I
                        53: invokevirtual #36                 // Method java/io/PrintStream.println:(I)V
                        56: return
                      LineNumberTable:
                        line 5: 0
                        line 6: 8
                        line 7: 13
                        line 8: 18
                        line 10: 28
                        line 11: 36
                        line 12: 41
                        line 13: 46
                        line 14: 56
                      LocalVariableTable:
                        Start  Length  Slot  Name   Signature
                            0      57     0  args   [Ljava/lang/String;
                            8      49     1    p1   Lbytecode/Pair;
                           36      21     2    p2   Lbytecode/Pair;
                }
                ```

              - 중요 부분
                - `//`이 있는 부분이 바로 심볼릭 레퍼런스이다
                - Pair 클래스 필드와 메소드 참조
                  - 클래스 : #17
                  - 필드
                    - a : #32
                  - 메소드
                    - setA() : #19
                    - setB() : #23
      - Heap Area
        - new 키워드로 생성한 객체를 저장하는 영역, GC의 대상

    - 각 쓰레드 별 생성

      - PC register
        - 현재 수행중인 명령어의 주소
        - 쓰레드 시작시 생성
      - Stack Area
        - 프로그램 실행시 사용
        - 메소드 호출시 마다 스택프레임(메소드 만을 위한 공간) 생성(push)
        - 메소드 정보(지역변수, 파리마터, 리턴값) 저장
        - 메소드 종료 후 삭제(pop) -> 메모리 반환
      - Native Method Stack
        - 자바 외의 언어(C, C++)로 작성된 네이티브 코드(기계어)를 실행하기 위한 영역
        - 주로 커널에 접근하는 코드가 이곳에 존재

- Execution Engine

  - byte code를 기계어(native code)로 변환 후 **명령어 실행**
  - 명령어 실행 방법
    - Interpreter 
      - 명령어 하나씩 변환해서 실행
      - 느리다
    - JIT 컴파일러
      - Interpreter 방식으로 실행하다가 특정 시간에 전체 byte code를 native code로 변환
        - 통째로 변환하므로 interpreter보다 느림
      - 변환한 native code는 캐시에 보관
        - 한번 컴파일된 native 코드는 다음에 빠르게 수행가능
        - 자주 실행되는 코드를 JIT로 변환하는게 좋다 
          - 실제로 실행 빈도수를 체크하여 일정 정도를 넘을 떄 컴파일을 수행한다
    - 비교
      - javac : source code -> byte code
      - JIT : byte code -> native code 
  - Garbage Collector
    - heap에 있는 객체 중 참조 되지 않는 객체 삭제

<br>

### GC의 동작 원리

**참고 자료**

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
    - 새로 생성한 객체가  Eden에 쌓임
    - Eden 영역에서 GC 발생, 살아 남은 객체는 Survivor영역중 하나로 이동
    - 살아남은 객체들이 Survivor에 지속적으로 쌓임
    - 하나의 Survivor 영역이 가득차게 되면 GC 발생, 살아남은 객체는 다른 Survivor 영역으로 이동(**age 증가**)
    - 이 과정을 지속적으로 반복하다 계속 살아 있는 객체들은 Old 영역으로 이동 

#### Major GC(Full GC)

- Old 영역에서 발생
- 종류
  - Serial GC
    - GC를 처리하는 쓰레드가 한 개, 시간이 오래 걸림
    - Mark-Sweap-Compaction
      - Mark and Sweap 하는건 동일
      - Compaction : 객체가 존재하는 부분과 없는 부분으로 나눔 -> 외부 단편화 문제 해결
  - Parallel GC
    - Serial GC와 동일, 단 GC 처리 쓰레드가 여러개
    - java8 default GC
  - CMS(Concurrent Mark Sweap) GC 
    - stop the world 시간 줄이기 위해서 나옴
    - 과정
  
      - Initial mark :  클래스 로더에서 가장 가까운 객체 중 살아있는 객체만 찾음 (stop the world - 짧음)
  
      - Concurrent mark : intial mark때 확인했던 **reachable한 객체들이 참조하는 객체들을 따라가며 마킹** (stop the world X (다른 쓰레드들도 진행 중))
      - Remark : Concurrent mark 단계에서 새로 추가되거나 참조가 끊긴 객체 확인 -> 최종적으로 reachable한 객체 확인 (stop the world)
      - Concurrent Sweap :  unreacable한 객체 제거 (stop the world X)
  
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
      - java9 부터 default GC

<br>

### 객체 지향 프로그래밍

- 컴퓨터 프로그래밍을 객체와 객체간의 상호작용으로 바라봄
- 구성 요소
  - 클래스 (현실 세계의 물체를 바라보듯이)
    - 속성(상태): 멤버변수, 필드
    - 행위: 메소드
  - 객체
    - 클래스의 인스턴스
    - 인스턴스 : 클래스에서 정의한 것을 실제 메모리에 할당
    - 객체의 생성과 초기화
      - 생성 : new 연산자 사용 -> heap에 객체 생성
      - 초기화 : 생성자에 의해 멤버 변수 초기화
      - 참고) 생성자
        - 생성자가 없을 때 컴파일러가 기본 생성자 자동 생성
        - 생성자가 따로 있는 경우는 자동 생성X
- 특징
  - strong cohesion : 한 모듈내에 존재하는 변수, 함수 사이의 밀접한 정도가 강해야함 (클래스가 담당한 기능만 확실하게 구현)
  - loose coupling : 서로 다른 모듈 간의 의존성은 느슨해야함 (의존성이 높아지면 수정시 고쳐야하는게 많음)
- 효과
  - 코드의 재사용성 증가
  - 코드의 유지보수 용이
  - 중복제거

<br>

### 객체 지향의 4가지 특징

- 추상화

  - 여러 클래스에 **공통적으로 사용**되는, 사용되어야 하는 내용(변수, 메소드)을 뽑아내는 것

  - 공통된 내용을 부모 클래스로 뽑아냄

- 캡슐화

  - 클래스에서 중요한 변수를 보존, 보호(변수에 유효한 값만 들어오게함) -> 메소드를 통해 변수에 접근
  - 예시
    - 멤버 변수의 접근 제어자 : private
    - getter(변수 획득), setter(변수 변경) 의 접근제어자 : public
    - 접근 제어자
      - private : 변수를 선언한 클래스에서만 접근 가능
      - (default) : 같은 패키지에 있는 클래스에서만 접근 가능
      - protected : 같은 패키지 또는 상속받은 클래스에서 접근 가능
      - public :  모든 클래스에서 접근 가능

- 상속

  - (부모)클래스의 멤버 변수와 메소드를 다른 (자식)클래스에게 물려주는 것 (재사용)
    - extends (클래스 상속), implements(인터페이스 상속)
  - **부모 객체 생성후 자식 객체 생성** -> 부모는 자식의 부분집합
  - 참고
    - super() : 부모 클래스의 생성자 호출
      - 부모 객체의 멤버 변수 초기화
      - **Object를 제외한 모든 클래스들은 super() 반드시 호출** 
    - super : 부모 객체 호출
      - static 메소드에서 사용 불가
    - this() : 같은 클래스내의 다른 생성자 호출
    - this : 객체 자신 호출
      - static 메소드에서 사용 불가
  - 장점
    - 코드의 재사용성이 증가
    - 확장성이 증가

- 다형성

  - **조상클래스 타입의 참조변수로 자손클래스의 인스턴스 참조**
  - 같은 타입의 인스턴스여도 참조 변수에 따라 사용할 수 있는 멤버가 다름
  - 형 변환 : 자식 타입 <- 부모 타입
  - **오버라이딩**
    - 자식 클래스에 부모 클래스의 메소드를 **재정의** 
    - 참조 변수가 아닌 **인스턴스의 메소드를 따라간다**
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
          super();// Parent클래스 생성 (이 경우 생략 가능)
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
          Child c2 = (Child) new Parent();// java.lang.ClassCastException 발생
      }
  }
  ```

<br>

### final static abstract

- final 
  - 클래스 : 상속 불가
  - 메소드 : 오버라이딩 불가
  - 변수 : 수정 불가(상수) -> 초기화 필요
- static
  - 클래스 (참고용)
    - static inner 클래스 : 부모 인스턴스 생성과는 상관없이 사용
    -  non-static inner클래스 : 부모 인스턴스 생성 후 사용가능
  - 메소드 : 멤버 변수 접근 불가 (클래스 메소드)
  - 변수 : 모든 객체에서 공유 (클래스 변수)
  - ex) `Math.min(4, 19)`
- abstract
  - 클래스 : 추상 클래스 - 클래스 내에 추상 메소드 존재가능
  - 메소드 : 추상 메소드 - 구현부가 없음 -> 자식클래스에서 오버라이딩

<br>

### 추상클래스와 인터페이스

**추상클래스**

- 추상클래스

  - 추상화(여러 객체간 공통된 내용 추출)를 통해 만들어진 클래스
  - 자식 클래스에서 상속 받아 확장하는 것이 목적
  - 인스턴스화 해서 사용 불가
- 추상메소드가 존재 가능
  - 미완성 설계도

- 추상메소드

  - 구현부가 없는 메소드
  - **반드시 자식 클래스에서 오버라이딩해서 구현**해야한다

**인터페이스**

- 인터페이스

  - **모든 메소드가 추상메소드** -> `public abstract`
    - abstact 키워드 생략 가능
- 멤버변수 :  `public static final`
- 인터페이스의 이해
  - 클래스를 사용하는 쪽(User)과 클래스를 제공하는 쪽(Provider)이 있다
  - 메소드를 사용(호출)하는 쪽(User)에서 사용하려는 메소드(Provider)의 선언부만 알면 된다
  - **즉, 역할(인터페이스)만 알면 되지 구체적으로 어떤 타입의 인스턴스가 들어올지는 신경 쓰지 않아도 된다**
  - 이를 통해 유연하게 확장할 수 있는 코드를 작성할 수 있음
  - 예시 : 운전자와 자동차

    - 운전자는 자동차를 운전하는 것에만 관심이 있다고하자
    - 운전자는 차종(아반떼, 테슬라, 벤츠, ....) 상관없이 자동차이기만 하면 된다

    - 즉, 구체(구체타입)적으로 어떤 것인지는 상관없고 역할(인터페이스)만 알면 된다

<br>

**추상클래스 vs 인터페이스**

- 상속
  - 추상클래스 : extends
  - 인터페이스 : implements
- 인터페이스만 다중 상속 가능

<br>

### 제네릭

진짜 어렵다...

- 제너릭이란
  - 타입을 일반화 시키는 것(임의의 타입 선언) 
  - 객체를 생성할 때 구체적인 타입 지정
  - 다양한 타입의 인스턴스 사용 가능

- 장점

  - 타입 안정성을 제공
    - **컴파일시에만 타입 체크** 
      - **인스턴스 생성시 지정한 구체적 타입으로 지정**
    - **런타임 시에는 (앞서 지정한 구체적인) 타입 정보를 가지고 있지 않음 -> 타입 소거** 
      - 런타임시 Object나 조상 클래스 타입이 들어올 수 있음
      - 컴파일시에 이미 타입 체크를 했으므로 안전
  - 타입체크와 형변환 생략 가능

- 제한이 없는 제네릭 클래스 (Unbound type)

  - **무공변**

  - 선언

    ```java
    class Box<T> {// T : 임의의 타입
        T item;
        
        void setItem(T item) { this.item = item; }
        T getItem() { return item; }
    }
    ```

    - 용어
      - `Box<T>` : 제네릭 클래스 
      - `T` : 타입 (매개)변수
      - `Box` : 원시 타입 (raw type)

  - 타입 소거 후

    ```java
    class Box {
        Object item;
        
        void setItem(Object item) { this.item = item; }
        Object getItem() { return item; }
    }
    ```

    - 타입 변수 T가 Object로 치환 

  - 제네릭 사용

    ```java
    Box<String> box = new Box<String>(); //객체 생성시 타입 지정
    box.setItem(new Object());// 컴파일 에러 : String 이외 타입 지정 불가
    box.setItem("ABC");
    String item = box.getItem();// 형변환 필요없음 : 컴파일시 타입 체크를 했기 때문
    ```

  - 제한

    - static

      - static은 모든 객체에 공유되어야 함
      - 하지만 제네릭을 통해 다양한 타입의 인스턴스 생성 가능 -> 사용 불가

    - 배열

      - 배열 vs 제네릭

        - 배열

          - 공변 : 구체적인 방향으로 타입 변환 허용 (다형성)

          - 구체화 타입(reifiable type) : 자신의 타입 정보를 런타임시에 알고 지킴

          - 코드

            ```java
            Object[] objArr = new Long[1];
            objArr[0] = "ABC";// 런타임 에러 발생
            ```

        - 제네릭

          - 무공변 : 변환 불가

          - 비구체화 타입(non-reifiable type) : 런타임시에 구체적인 타입 정보가 없음(타입입 소거 때문)

          - 코드

            ```java
            List<Object> objList = new ArrayList<Long>();// 컴파일 에러 발생
            objList.add("ABC");
            ```

        - 결론

          - 제네릭 클래스에 배열이 멤버 변수로 있다면
            - 제네릭은 컴파일 시 (<u>객체 생성 시 지정한 구체적</u>) 타입 체크 후 타입 소거함
            - 배열은 런타임시 타입 체크를 해야되는데 (구체적인) 타입 정보가 없음
            - 오류 발생

- 제네릭 클래스의 객체 생성과 사용

  - 원시 타입과 타입 변수로 나눠서 생각하자

    - 기본적으로 `Box<Fruit> box` 로 변수가 선언되어 있다면 
      - 인스턴스로 `new Box<Fruit>()` 만 사용 가능 ( 또는 `new Box<>()`)
    - 원시타입은 다형성 가능(타입변수가 같을때), 타입 변수는 불가능

  - 예시

    - 세팅

      ```java
      class Box<T> {
      	ArrayList<T> list = new ArrayList<>();
      	
      	void add(T item) { list.add(item); }
      }
      
      class FruitBox<T> extends Box<T> {}
      class Fruit {public String toString() {return "Fruit";}}
      class Apple extends Fruit {public String toString() {return "Apple";}}
      ```

    - 실행

      ```java
      Box<Apple> appleBox = new FruitBox<>();// 클래스는 상속 가능
      FruitBox<Fruit> fruitBox = new FruitBox<Apple>(); // 컴파일 에러 : 선언시 정한 타입 파라미터만 사용 가능
      
      FruitBox<Fruit> fruitBox2 = new FruitBox<>();
      fruitBox2.add(new Fruit());
      fruitBox2.add(new Apple());//add 메소드의 인자 타입이 Fruit이므로 사용 가능
      ```

- 제한된 제네릭 클래스 (Bounded type)

  - 타입 변수를 제한

  - `<T extends XXX>` 

    - T는 XXX 또는 자손 타입으로 제한
    - 인터페이스도 `extends` 사용
    - 다중 제한 
      - `&` 사용
      - `<T extends XXX & YYY>` : XXX의 자손이면서 동시에 YYY의 자손인 타입만 사용가능 

  - 선언

    ```java
    // 기존 FruitBox의 타입 변수를 Fruit로 제한
    class FruitBox<T extends Fruit> extends Box<T> {
        List<T> list = new ArrayList<>();
        
        void add(T fruit) {
            list.add(fruit);
        }
        
        T get(int i) {
            return list.get(i);
        }
    }
    ```
    
  - 타입 소거 후

    ```java
    class FruitBox extends Box {
        List list = new ArrayList();
        
        void add(Fruit fruit) {// T가 Fruit로 치환
            list.add(fruit);
        }
        
        Fruit get(int i) {// T가 Fruit로 치환
            return (Fruit) list.get(i); // 형변환 (Object타입을 반환하므로) 
        }
    }
    ```

- 와일드 카드

  - 제네릭 클래스의 무공변에서 오는 답답함 해결

  - 타입 변수의 범위를 지정하여 다양한 타입을 갖는 객체 생성 가능

  - 종류

    - `<? extends T>` : 상한 제한, T와 그 자손들만 가능

      - **공변 : 추상 -> 구체** 
      - **읽기만 가능**

    - `<? super T>` : 하한 제한, T와 그 조상들만 가능

      - **반공변 : 구체 -> 추상** 
      - **쓰기만 가능**

    - `<?>` : 제한 없음, 모든 타입 가능, `<? extends Object>` 와 동일

    - extends와 super의 비교 -> 기준 : 다형성

      ```java
      public void doSomething(List<? extends MyClass> list) {
          for (MyClass e : list) { // Ok (조상 타입(MyClass)으로 구체 타입(?) 참조 가능)
              System.out.println(e);
          }
      }
      
      public void doSomething(List<? extends MyClass> list) {
          list.add(new MyClass()); // Compile Error (구체 타입(?)으로 조상 타입(MyClass) 참조 불가)
      }
      
      public void doSomething(List<? super MyClass> list) {
          for (MyClass e : list) { //  Compile Error (구체 타입(MyClass)으로 조상 타입(?) 참조 불가)
              System.out.println(e);
          }
      }
      
      public void doSomething(List<? super MyClass> list) {
          list.add(new MyClass()); // Ok (조상 타입(?)으로 구체 타입(MyClass) 참조 가능)
      }
      ```

  - 예시 - 과일 종류 별로 주스 만들기

    - 원시타입이 같아도 타입 변수가 다른 경우는 오버로딩 불가능 -> 타입소거로 인해

      ```java
      class Juicer {// 주스기계
          // 아래 두 메소드는 서로 같은 메소드 취급 - 컴파일 에러 발생
          // static 메소드가 사용되었음에 유의
      	static Juice makeJuice(FruitBox<Fruit> box) {
      		String tmp = "";
      		for (Fruit f : box.list) tmp += f;
      		return new Juice(tmp);
      	}
      	
      	static Juice makeJuice(FruitBox<Apple> box) {
      		String tmp = "";
      		for (Fruit f : box.list) tmp += f;
      		return new Juice(tmp);
      	}
      }
      ```

    - 와일드카드 사용

      ```java
      class Juicer {
          // static 메소드가 사용되었음에 유의
          // Fruit와 하위 타입을 인자로 받을 수 있다
      	static Juice makeJuice(FruitBox<? extends Fruit> box) {
      		String tmp = "";
      		for (Fruit f : box.list) tmp += f;// FruitBox의 제네릭 타입은 Fruit로 제한되어 있기 떄문에 사용 가능
      		return new Juice(tmp);
      	}
      }
      ```

- 제네릭 메소드
  
  - 메소드 선언부에 제네릭 타입이 선언
  
  - **제네릭 클래스에 정의된 타입 변수와는 별개**
  
    - 사용법은 제네릭 클래스와 거의 동일
    - `<T>` 와 `<T extends XXX>` 사용가능
  
  - **해당 메소드에서 지역 변수처럼 사용**
  
    - 매개 변수 타입
    - 리턴 타입
  
  - 사용 예시
  
    - 과일 종류 별로 주스 만들기 -> 다형성 지원
  
      - 세팅
  
        ```java
        class Juicer {
            // static 메소드임에 유의
            static <T extends Fruit> Juice makeJuice(FruitBox<T> box) {
        		String tmp = "";
        		for (Fruit f : box.list) tmp += f;
        		return new Juice(tmp);
        	}
        }
        ```
  
      - 실행
  
        ```java
        FruitBox<Fruit> fruitBox = new FruitBox<>();
        FruitBox<Apple> appleBox = new FruitBox<>();
        
        System.out.println(Juicer.<Fruit>makeJuice(fruitBox));
        System.out.println(Juicer.makeJuice(fruitBox));// 타입 추론
        System.out.println(Juicer.<Apple>makeJuice(appleBox));
        System.out.println(Juicer.makeJuice(appleBox));// 타입 추론
        ```
  
    - `Collections.sort()` 메소드
  
      - 구조
  
        ```java
        public static <T extends Comparable<? super T>> void sort(List<T> list) {
            list.sort(null);
        }
        ```
  
      - 의미
        - list의 타입 :  T
        - T는 Comparable을 구현한 클래스
        - Comparable의 타입은 T와 그 조상
  
- 제네릭의 형변환

  - 타입 변수의 타입이 다르면 형변환 불가

    ```java
    Box<Object> objBox = null;
    Box<String> strBox = null;
    
    objBox = (Box<Object>) strBox;// 형변환 불가
    strBox = (Box<String>) objBox;// 형변환 불가
    ```

  - 와일드카드를 사용한 형변환

    ```java
    Box<? extends Object> objBox1 = new Box<String>();// 형변환 가능
    
    FruitBox<? extends Fruit> box1 = new FruitBox<Fruit>();// 형변환 가능
    FruitBox<? extends Fruit> box2 = new FruitBox<Apple>();// 형변환 가능
    
    Box<?> extends objBox2 = new Box<?>();// 타입이 정해지지 않았으므로 불가능
    Box<?> extends objBox3 = new Box<>();// 가능(?는 ? extends Object와 동일)
    ```

<br>

### 불변 객체

- 참고자료

  - [conatuseus.log - [Java] Immuatable Objec(불변객체)](https://velog.io/@conatuseus/Java-Immutable-Object%EB%B6%88%EB%B3%80%EA%B0%9D%EC%B2%B4)
  - [자주와조요 - 불변 객체란?](https://velog.io/@kyle/%EB%B6%88%EB%B3%80-%EA%B0%9D%EC%B2%B4%EB%9E%80-Java-Immutable-Object)

- 불변 객체란

  - 한 번 생성 후, 외부에서 변경 불가능한 객체

- 장단점

  - 장점(사용목적)
    - 객체 신뢰도 증가 : transaction 내에서 신뢰하고 쓸 수 있음
    - 생성자 및 접근 메소드(ex) getter)의 복사에 대한 방어 필요 없음
    - 멀티스레드 환경에서 동기화 처리없이 객체 공유
  - 단점
    - 매번 새로운 객체가 필요 -> 메모리 누수 및 성능저하 발생

- 구현

  - 공통 사항

    - 모든 필드는 private 제어자 사용
      - 외부에서 필드 수정 직접 하는 것 불가
    - setter 사용X
      - 필드 값 수정 불가 
    - final 사용 권장
      - final : 한 번만 할당 가능
      - final 키워드가 붙은 변수는 setter 생성 불가

  - 필드가 primitive type만 있는 경우 : setter사용X, final 사용

    ```java
    // 1. 필드가 기본형 타입만 있는 경우
    class PrimitiveObject {
    	private final int v;
    
    	public PrimitiveObject(int v) {
    		this.v = v;
    	}
    
    	public int getV() {
    		return v;
    	}
    }
    ```

  - 필드에 reference type이 있는 경우

    - 단일 객체만 있는 경우 : 해당 객체도 불변 객체로 생성

      ```java
      // 2. 필드에 참조형 타입(단일 객체)이 있는 경우
      class ReferenceObject {
      	private final Pair p;
      
      	public ReferenceObject(final Pair p) {
      		this.p = p;
      	}
      
      	public Pair getPair() {
      		return p;
      	}
      }
      
      // 참조 변수도 불변 객체여야 한다
      class Pair {
      	private final int r, c;
      
      	public Pair(int r, int c) {
      		this.r = r;
      		this.c = c;
      	}
      
      	public int getR() {
      		return r;
      	}
      
      	public int getC() {
      		return c;
      	}
      
      	@Override
      	public String toString() {
      		return "Pair [r=" + r + ", c=" + c + "]";
      	}
      }
      ```

    - 배열이 있는 경우 

      - 같은 인스턴스를 가리키는 것을 막아야한다
        - 외부에서 인덱스 값을 새로 **재할당** 할 수 있기 때문이다 -> 변경 가능
      - 구현 - 깊은 복사 
        - 필드 초기화 : 인자로 들어오는 배열의 요소를 가지는 **새로운 배열 객체 생성**
        - 필드 접근 : 외부에서 수정 불가능하게 **원본과 다른 복사본을 생성**해서 리턴

      ```java
      // 3. 필드에 배열이 있는 경우
      class ArrayObject {
      	private final Pair[] array;
      
      	public ArrayObject(Pair[] array) {
      //		this.array = array; // 문제 발생 : 파라미터로 받은 array와 필드 array가 같은 인스턴스를 가리킨다(주소가 같다)
      		this.array = Arrays.copyOf(array, array.length); // 해결 : 복사본 생성 (깊은 복사, 서로 다른 주소 참조)
      	}
      
      	public Pair[] getArray() {
      //		return array; // 문제 발생 : 외부에서 할당한 참조 변수와 필드 array가 같은 인스턴스를 가리킨다(주소가 같다) 
      		return (array == null) ? null : array.clone(); // 해결 : 복사본 생성 (깊은 복사, 서로 다른 주소 참조)
      	}
      }
      ```

    - List가 있는 경우

      - 배열과 같은 방식으로 구현한다
      - 구현 - 깊은 복사
        - 필드 초기화 : 인자로 들어오는 List의 요소를 가지는 **새로운 List 객체 생성**
        - 필드 접근 : 외부에서 수정 불가능하게 `Collections.unmodifiableList(List list);` 사용
      
      ```java
      // 4. 필드에 리스트가 있는 경우
      class ListObject {
      	private final List<Pair> list;
      
      	public ListObject(List<Pair> list) {
      //		this.list = list; // 문제 : 필드에 리스트가 있는 경우와 동일
      		this.list = new ArrayList<>(list); // 해결
      	}
      
      	public List<Pair> getList() {
      //		return list; // 문제 발생
      		return Collections.unmodifiableList(list);// 해결
      	}
      } 
      ```

  - 테스트

    ```java
    public class ImmutableObjectTest {
    	public static void main(String[] args) {
    		PrimitiveObject primitiveObject = new PrimitiveObject(10);
    		System.out.println(primitiveObject.getV());
    		
    		ReferenceObject referenceObject = new ReferenceObject(new Pair(4, 5));
    		referenceObject.getPair();
    		
    		Pair[] pairArray = new Pair[3];
    		pairArray[0] = new Pair(1, 2);
    		pairArray[1] = new Pair(2, 3);
    		pairArray[2] = new Pair(3, 4);
    		
    		ArrayObject arrayObject = new ArrayObject(pairArray);
    		Pair[] getArray = arrayObject.getArray();
    		getArray[0] = new Pair(4, 5);
    		
    		System.out.println(Arrays.toString(getArray));// 수정X
    		System.out.println(Arrays.toString(pairArray));// 수정O
    		System.out.println(Arrays.toString(arrayObject.getArray()));// 수정X
    		
    		List<Pair> pairList = new ArrayList<Pair>();
    		pairList.add(new Pair(1, 2));
    		pairList.add(new Pair(2, 3));
    		pairList.add(new Pair(3, 4));
    		
    		ListObject listObject = new ListObject(pairList);
    		List<Pair> getList = listObject.getList();
    		getList.add(new Pair(4, 5)); // 에러 발생
    		
    		System.out.println(getList);
    		System.out.println(pairList);
    		System.out.println(listObject.getList());
    	}
    }
    ```

- 요약

  - 기본: private 접근 제어자, setter 사용X(final 사용)

  - primitive type
    - 기본대로
  - referrence type
    - 단일 객체 : 참조하는 객체도 기본대로 불변객체로 생성
    - 배열, 리스트
      - 새로운 문제 발생
        - 원인 : 같은 인스턴스 주소를 가리켜서 재할당시 변경 발생
      - 해결
        - 주소를 다르게 한다 -> 깊은 복사
          - 필드 초기화
            - 배열, 리스트 모두 깊은 복사한 새로운 인스턴스 생성
          - 필드 접근 (getter)
            - 배열 : 깊은 복사한 새로운 인스턴스 반환
            - 리스트 : `Collections.unmodifiableList()` 사용하여 변경 시 예외 발생시킴

<br>

### 예외처리

- 참고 자료
  
  - 자바의 정석
  - [기적을 만드는 기록 - 예외 & 예외 처리란?](https://jiwontip.tistory.com/5)
  
- 에러와 예외
  - 에러 : 수습 불가능한 오류
  - 예외 : 수습가능한 다소 미약한 오류
  
- 예외 클래스의 계층 구조
  -  Throwable
     -  Exception
         -  (Checked Exception) - compile 시 확인 (외적 요인에 의해 발생) => 예외 처리 안하면 compile 오류 발생
             -  IOException
             -  ....
             -  ClassNotFoundException
         -  (Unchecked Exception) RuntimeException - runtime 시 확인 (프로그래머 실수로 발생)
             -  ArithmeticException
             -  NullPointerException
             -  ...
             -  IndexOutOfBoundException

- 예외처리 목적

  - **프로그램의 비정상적인 종료를 막고, 정상적인 실행상태 유지**

- 예외 처리 방법

  - try-catch-finally

    ```java
    try {
        // 예외를 처리하길 원하는 실행 코드 (예외가 발생할 수 있는 코드)
    } catch (XxxException e1) {
        // e1 예외가 발생시 실행되는 코드
        // e1.printStackTrace();
        // e1.getMessage();
    } catch (Yyy Exception e2) {
        // e2 예외가 발생시 실행되는 코드
    } final
        // 예외 발생 여부와 관계 없이 실행, 마지막에 반드시 실행 시키야되는 코드가 있을 떄 사용
    }
    ```

    - 흐름
      - try 블럭에서 예외가 발생한 경우
        - **더이상 try 블럭의 코드가 실행되지 않음**
        - 발생한 예외에 해당하는 인스턴스 생성
        - 해당 인스턴스의 타입을 매개변수로하는 catch 블럭이 있는지 **위에서 부터 순서대로** 확인 -> 정확히는 `instanceof` 로 타입 검사
          - 있다면 : 예외처리O -> try-catch 블럭을 빠져나옴
          - 없다면 : 예외처리X -> 비정상 종료
      - 예외가 발생하지 않은 경우
        - catch블럭 거치지 않고(try 블럭만 실행하고), try-catch 블럭을 빠져나옴
    - `printStackTrace()` 와 `getMessage()`
      - `printStackTrace()`  : 예외메시지와 호출 스택에 있던 메소드 정보 출력
      - `getMessage()` : 예외 인스턴스에 저장된 메시지 출력

  - throw, throws

    - throw : 강제로 예외 발생 시키기
    - throws 
      - 메소드에서 발생할 수 있는 예외 명시
        - 해당 해소드를 호출한 상위 메소드로 예외 넘김
      - 반드시 상위 메소드 중 하나에서 예외 처리를 해야함
        - try-catch 사용해서 처리

  - 사용자 정의 예외 클래스 

    - Exception이나 RuntimeException 클래스를 상속받아 구현 
    - Exception을 상속하는 경우 예외 처리를 안하면 컴파일 에러 발생
    - RuntimeException을 상속하는 경우 예외처리를 안해도 컴파일 에러 발생X

  - 구현 예시

    - 사용자 정의 예외 - RuntimeException 상속

      ```java
      class StackisFullException extends RuntimeException {
      	public StackisFullException() {
      		super("Stack is Full");
      	}
      }
      
      class StackisEmptyException extends RuntimeException {
      	public StackisEmptyException() {
      		super("Stack is Empty");
      	}
      }
      ```

    - 스택

      ```java
      class MyStack {
      	int[] arr;
      	int top;
      	
      	public MyStack(int size) {
      		this.arr = new int[size];
      		this.top = 0;
      	}
      	
          // RuntimeException의 경우 throws를 명시 안해줘도 되긴함, Exception은 반드시 throws 명시
      	public void push(int data) throws StackisFullException {
      		if (top == arr.length) {
      			throw new StackisFullException();
      		}
      		System.out.println("push : "+data);
      		arr[top++] = data;
      	}
      	
      	public int pop() throws StackisEmptyException {
      		if (top == 0) throw new StackisEmptyException();
      		top--;
      		System.out.println("pop : "+arr[top]);
      		return arr[top];
      	}
      }
      ```

    - 예외 처리 - main

      ```java
      public class ExceptionTest {
      	public static void main(String[] args) {
      		System.out.println("start main");
      		MyStack myStack = new MyStack(3);
      		
      		System.out.println("--------------------------------------------");
      		try {
      			myStack.push(1);
      			myStack.push(2);
      			myStack.pop();
      			myStack.pop();
      			
      			myStack.push(3);
      			myStack.push(4);
      			myStack.push(5);
      			myStack.push(6); // StackisFullException 발생
      			
      			myStack.pop();
      			myStack.pop();
      			myStack.pop();
      		} catch (StackisFullException | StackisEmptyException e) {
      			e.printStackTrace();
      		}
      		
      		System.out.println("--------------------------------------------");
      		
      		myStack = new MyStack(3);
      		try {
      			myStack.push(1);
      			myStack.push(2);
      			myStack.pop();
      			myStack.pop();
      			myStack.pop();// StackisEmptyException 발생
      			
      			myStack.push(3);
      			myStack.push(4);
      			myStack.push(5);
      		} catch (StackisFullException | StackisEmptyException e) {
      			e.printStackTrace();
      		}
      		
      		System.out.println("--------------------------------------------");
      		System.out.println("end main");
      	}
      }
      ```

    - 실행 결과

      ```java
      start main
      --------------------------------------------
      push : 1
      push : 2
      pop : 2
      pop : 1
      push : 3
      push : 4
      push : 5
      exceptionhandling.StackisFullException: Stack is Full
      	at exceptionhandling.MyStack.push(ExceptionTest.java:62)
      	at exceptionhandling.ExceptionTest.main(ExceptionTest.java:21)
      --------------------------------------------
      push : 1
      push : 2
      pop : 2
      pop : 1
      exceptionhandling.StackisEmptyException: Stack is Empty
      	at exceptionhandling.MyStack.pop(ExceptionTest.java:69)
      	at exceptionhandling.ExceptionTest.main(ExceptionTest.java:37)
      --------------------------------------------
      end main
      ```

      - 예외가 처리되어 정상적으로 프로그램 종료

- 요약

  - 에러 vs 예외
    - 에러 : 수습 불가능
    - 예외 : 수습 가능 -> 예외처리
    - 에러와 예외가 발생하면 프로그램이 비정상으로 종료된다
  - 예외 처리
    - 예외가 발생해도 프로그램이 종료되지 않고 정상적으로 동작하게 하게 처리
    - 방법
      - `try-catch`
        - try 블럭
          - 예외가 발생할 수 있는 코드 기술
          - **예외가 발생한다면 try 블럭은 더이상 실행X**
        - catch 블럭 
          - try에서 발생한 예외 처리
          - try 블럭에서 발생한 예외 인스턴스의 타입을 가지는 catch 블럭을 찾아 예외 처리
        - finally 블럭
          - 예외 발생 여부와 상관없이 항상 실행
          - 마지막에 반드시 실행시켜야 하는 코드가 있을 때 사용
      - `throw` 와 `throws`
        - `throw` 
          - 강제로 예외 발생
        - `throws` 
          - 현재 메소드에서 발생한 예외를 상위 메소드로 전달
            - 반드시 상위 메소드 중 한 곳에서 예외처리(`try-catch`)를 해줘야 함
          - 메소드에서 발생할 수 있는 예외를 명시적으로 나타내는 용도로도 사용
            - `Exception` 은 반드시 `throws` 명시
            - `RuntimeException`의 경우 `throws`를 명시 안해줘도 예외 처리 가능

<br>

### 문자열 처리

#### String

- 선언 : 내부적으로 char[]가 생성

- 불변 **객체**

  - 변경 불가능
  - 읽기만 가능

- 문자열 결합

  ```java
  String a = "a";
  String b = "b";
  a = a+b;// a에 "b"가 더해지는게 아닌 "ab"를 가지는 인스턴스 생성
  ```

- 문자열 리터럴

  ```java
  String a = "abc";
  String b = "abc";
  // a와 b의 주소는 같다
  ```

#### StringBuffer와 StringBuilder

- StringBuffer

  - 생성

    ```java
    public StringBuffer() {// 인자 없는 경우 capacity 16으로 초기화
        super(16);
    }
    
    public StringBuffer(int capacity) {
        super(capacity);
    }
    
    public StringBuffer(CharSequence seq) {// capacity를 초과한 경우 +16해줌
        this(seq.length() + 16);
        append(seq);
    }
    ```

  - 문자열 결합

    ```java
    StringBuffer sb = new StringBuffer();
    sb.append("a").append("b").append("asddasd");
    ```

    - append()의 리턴 값은 StringBuffer의 주소
    - **String은 새로운 문자열 리터럴이 생성될떄 마다 객체를 새로 생성, 반면 StringBuffer는 처음에 선언시에만 객체 생성**
    - 문자열 결합이나 추출이 많을 경우 String보다 StringBuffer를 활용

- StringBuilder

  - StringBuffer랑  똑같음
  - 차이점
    - StringBuffer : 동기화 지원 -> 멀티쓰레드인 경우 사용
    - StringBuilder : 동기화 지원X -> 싱글쓰레드인 경우 사용

<br>

### 직렬화

- 참고 자료
  - [Nesoy Blog - Java의 직렬화란?](https://nesoy.github.io/articles/2018-04/Java-Serialize)
  - [Stop the world - Java 객체 직렬화와 역직렬화](https://flowarc.tistory.com/entry/Java-%EA%B0%9D%EC%B2%B4-%EC%A7%81%EB%A0%AC%ED%99%94Serialization-%EC%99%80-%EC%97%AD%EC%A7%81%EB%A0%AC%ED%99%94Deserialization)
  - Java의 정석 - 직렬화Part (p.934~p.944)
  - [madplay - [이펙티브 자바 3판] 12장. 직렬화](https://madplay.github.io/post/effectivejava-chapter12-serialization)
  
- 직렬화, 역직렬화

  - 직렬화의 목적
    - 자바 시스템간에 객체를 저장하거나 전송하기 위함
  - 직렬화 
    - 객체 -> 데이터 스트림(바이트 코드)
      - <u>객체 그래프</u>를 따라가며 진행
    - **인스턴스 변수만 직렬화의 대상(클래스 변수는 대상x)**
  - 역직렬화 
    - 데이터 스트림 -> 객체
  - `Object` 타입 객체는 직렬화 불가

- 구현

  - 직렬화

    - 조건 - `java.io.Serializable` 구현 (참고 : 부모 객체가 직렬화 가능하면 자식 객체도 가능)

      ```java
      class UserInfo implements Serializable {
      	private String name;
      	private transient String password;// transient : 직렬화 대상에서 제외
      	private int age;
      	
      	public UserInfo(String name, String password, int age) {
      		this.name = name;
      		this.password = password;
      		this.age = age;
      	}
      
      	@Override
      	public String toString() {
      		return "UserInfo [name=" + name + ", age=" + age + "]";
      	}
      }
      ```

    - 방법 - `java.io.ObjectOutputStream` 사용

      ```java
      import java.io.BufferedOutputStream;
      import java.io.FileOutputStream;
      import java.io.IOException;
      import java.io.ObjectOutputStream;
      import java.io.Serializable;
      import java.util.ArrayList;
      
      public class SerializeTest {
      	public static void main(String[] args) {
      		try {
      			String fileName = "UserInfo.dat";
      			FileOutputStream fos = new FileOutputStream(fileName);
      			BufferedOutputStream bos = new BufferedOutputStream(fos);
      			
      			ObjectOutputStream oos = new ObjectOutputStream(bos);
      			
      			UserInfo u1 = new UserInfo("beaver1", "1234", 32);
      			UserInfo u2 = new UserInfo("beaver2", "4321", 13);
      			ArrayList<UserInfo> list = new ArrayList<>();
      			list.add(u1);
      			list.add(u2);
      			
      			// 직렬화 수행
      			oos.writeObject(u1);
      			oos.writeObject(u2);
      			oos.writeObject(list);
      			oos.close();
      			System.out.println("직렬화 종료");
      		} catch (IOException e) {
      			e.printStackTrace();
      		}
      	}
      }
      ```

    - 역직렬화

      - 조건

        - 직렬화 했을 때와 같은 클래스를 사용해야함, 같은 클래스더라도 클래스의 내용이 변경된 경우 역직렬화 실패
        - 직렬화와 역직렬화를 진행하는 시스템이 서로 다를 수 있음을 인지
        - 동일한 `serialVersionUID`를 가지고 있어야 함
          - `serialVersionUID` : 클래스 버전(기본값 : 클래스의 기본 해쉬값)
          - `serialVersionUID`는 클래스가 변경(메소드 or 필드 변경) 될 때마다 변경
          - `serialVersionUID`가 다를 경우 `java.io.InvalidClassException` 발생
        - **직렬화할 때와 역직렬화 할 때의 순서는 동일해야 함**
          - List에 데이터를 저장하는것이 편하다
        
      - 방법 - `java.io.ObjectInputStream` 사용
      
        ```java
        import java.io.BufferedInputStream;
        import java.io.FileInputStream;
        import java.io.ObjectInputStream;
        import java.util.ArrayList;
        
        public class DeserializeTest {
        	public static void main(String[] args) {
        		try {
        			String fileName = "UserInfo.dat";
        			FileInputStream fis = new FileInputStream(fileName);
        			BufferedInputStream bos = new BufferedInputStream(fis);
        			
        			ObjectInputStream ois = new ObjectInputStream(bos);
        			
        			UserInfo u1 = (UserInfo) ois.readObject();
        			UserInfo u2 = (UserInfo) ois.readObject();			
        			ArrayList<UserInfo> list = (ArrayList) ois.readObject();
        			
        			System.out.println(u1);
        			System.out.println(u2);
        			System.out.println(list);
        			ois.close();
        			System.out.println("역직렬화 종료");
        		} catch (Exception e) {
        			e.printStackTrace();
        		}
        	}
        }
        ```
      
      -  `serialVersionUID` 적용
      
          - **상황 - class의 구성 요소 변경**
      
            ```java
            class UserInfo implements Serializable {
              String name;
              transient String password;
              int age;
              String tel;// 중간에 추가된 필드
      
              public UserInfo(String name, String password, int age) {
                this.name = name;
                this.password = password;
                this.age = age;
              }
      
              @Override
              public String toString() {
                return "UserInfo [name=" + name + ", age=" + age + "]";
              }
            }
            ```
      
          - 실행 결과 : `java.io.InvalidClassException` 발생
      
          - 해결  :  클래스에 `serialVersionUID` 명시 
      
            ```java
            class UserInfo implements Serializable {
              /**
               * 
               */
              private static final long serialVersionUID = -1224074296460704595L;;//SUID 명시
            
              String name;
              transient String password;
              int age;
              String tel; 
            
              public UserInfo(String name, String password, int age) {
                this.name = name;
                this.password = password;
                this.age = age;
              }
            
              @Override
              public String toString() {
                return "UserInfo [name=" + name + ", age=" + age + "]";
              }
            }
            ```

- 직렬화의 단점
  - 다른 포맷(ex) JSON)에 비해 큰 용량
  - 위험함
    - 역직렬화시 사용되는 `ObjectOutputStream` 의 `readObject()` 메소드
      - 매개변수로 바이트 스트림을 받는 **생성자**
      - 바이트 스트림의 불변성이 깨지면 문제 발생
      - 해결(순서대로 수행)
        - 방어적 복사 : 복사본을 리턴해서 클라이언트 측에서 수정해도 원본 객체의 값은 변하지 않게함
        - 유효성 검사 : 데이터 무결성 확인
  - 대안 : XML, JSON, ...

- 요약

  ![img](https://postfiles.pstatic.net/MjAxNzExMTdfOTMg/MDAxNTEwODk4OTAzOTgx.-b5ljPnLfaNGLZdcPkUZ2_LoS-5-BLxRfxzwlm2qsSMg.YcnYVCUFjqa7lyTj8osxs3-g4YNLn29D0iVMq-kpyhcg.PNG.kkson50/%EC%A7%81%EB%A0%AC%ED%99%94_serialization_%EC%97%AD%EC%A7%81%EB%A0%AC%ED%99%94_serialversionuid%EC%B2%B4%ED%81%AC.png?type=w2)



<br>

### Comparable과 Comparator

- 둘 다 인터페이스 이다 (함수형 인터페이스)
  - 자식클래스에서 상속해서 구현
  - wrapper클래스, String클래스 등에서 이를 상속하고 있음

- 객체의 정렬을 위해서 사용 -> 비교를 통해 정렬
  - Comparable 
    - 기본 정렬 기준을 구현 (클래스 내에서 구현) -> `Colletions.sort(list)`
    - 메소드 : compareTo()
  - Comparator 
    - 기본 정렬 기준 외에 다른 기준으로 정렬 -> `Collections.sort(list, ComparatorObj)`
    - 메소드 : compare()
  
- 코드

  ```java
  // Comparator와 Comparable로 내림차순 정렬
  public class CompareTest {
  	public static void main(String[] args) {
  		List<Pair> list = new ArrayList<>();
  		list.add(new Pair(1, "강호동"));
  		list.add(new Pair(2, "유재석"));
  		list.add(new Pair(3, "박명수"));
  		
  		// id의 내림차순으로 정렬
  		Collections.sort(list);
  		System.out.println(list);
  		
  		// 이름의 내림차순으로 정렬
  		Collections.sort(list, new Descending());
  		System.out.println(list);
  	}
  	
  	static class Pair implements Comparable<Pair>{
  		int id;
  		String name;
  
  		public Pair(int id, String name) {
  			this.id = id;
  			this.name = name;
  		}
  
  		@Override
  		public int compareTo(Pair p) {
  			return (this.id - p.id) * -1;
  		}
  
  		@Override
  		public String toString() {
  			return "[id=" + id + ", name=" + name + "]";
  		}
  	}
  	
  	static class Descending implements Comparator<Pair>{
  		
  		@Override
  		public int compare(Pair p1, Pair p2) {
  			return p1.name.compareTo(p2.name) * -1;
  		}
  	}
  }
  ```

<br>

### 익명 클래스

- 익명 클래스란?

  - 선언과 생성을 동시에 수행
  - 어떤 클래스를 상속하거나 인터페이스를 구현하여 **일회성으로 사용**
  - getClass()로 어떤 클래스인지 확인할때 클래스명이 나오지 않음
    - `외부클래스명$숫자.class`

- 구현

  - 방법

    ```java
    new 조상클래스이름() {
        // 구현
    }
    
    new 구현인터페이스이름() {
        // 구현
    }
    ```

    - 클래스 선언과 동시에 객체 생성
    - 생성자 사용 불가
    - 단일 상속만 가능

  - 코드

    ```java
    public class AnonymousClassTest {
    	public static void main(String[] args) {
    		Car car1 = new Car();
    		Car car2 = new Benz();
    		//익명 클래스 생성
    		Car car3 = new Car() {
    			public void showName() {
    				System.out.println("Benz");
    			};
    		};
    		
    		//메소드 실행
    		car1.showName();
    		car2.showName();
    		car3.showName();
    		
    		//클래스 확인
    		System.out.println(car1.getClass());
    		System.out.println(car2.getClass()); 
    		System.out.println(car3.getClass()); 
    	}
    }
    
    
    class Car {
    	public void showName() {
    		System.out.println("no name");
    	}
    }
    
    class Benz extends Car{
    	public void showName() {
    		System.out.println("Benz");
    	}
    }
    
    ```

  - 실행 결과

    ```
    no name
    Benz
    Benz
    class anomymous.Car
    class anomymous.Benz
    class anomymous.AnonymousClassTest$1
    ```

- 익명 인터페이스

  - 코드

    ```java
    public class AnonymousInterfaceTest {
    	public static void main(String[] args) {
    		//익명 인터페이스 구현
    		RemoteController remoteController = new RemoteController() {
    			@Override
    			public void turnOn() {
    				System.out.println("turn on");
    			}
    			
    			@Override
    			public void turnOff() {
    				System.out.println("turn off");
    			}
    		};
    		
    		//메소드 실행
    		remoteController.turnOn();
    		remoteController.turnOff();
    		//클래스 확인
    		System.out.println(remoteController.getClass());
    		System.out.println();
    		
    		
    		Animal lion1 = new Lion();
    		//익명 인터페이스 구현
    		Animal lion2 = new Animal() {
    			@Override
    			public void move() {
    				System.out.println("Lion is moving");
    			}
    		};
    		Animal lion3 = () -> {System.out.println("Lion is moving");};// 람다식이용
    		
    		//메소드 실행
    		lion1.move();
    		lion2.move();
    		lion3.move();
    		
    		//클래스 확인
    		System.out.println(lion1.getClass());
    		System.out.println(lion2.getClass());
    		System.out.println(lion3.getClass());
    	}
    }
    
    interface RemoteController {
    	public void turnOn();
    	public void turnOff();
    }
    
    //함수형 인터페이스 : 한 개의 추상메소드만 있음 -> 람다식으로 구현 가능
    interface Animal {
    	public void move();
    }
    
    class Lion implements Animal {
    
    	@Override
    	public void move() {
    		System.out.println("Lion is moving");
    	}
    }
    ```

  - 실행결과

    ```
    turn on
    turn off
    class anomymous.AnonymousInterfaceTest$1
    
    Lion is moving
    Lion is moving
    Lion is moving
    class anomymous.Lion
    class anomymous.AnonymousInterfaceTest$2
    class anomymous.AnonymousInterfaceTest$$Lambda$1/0x0000000800061040
    ```

  <br>

### 람다식

참고 자료 : 자바의 정석 p.794~811

- == 익명 객체

- 람다식으로 전환 하기

  - 메소드

    ```java
    int max(int a, int b) {
    	return a > b ? a : b;
    }
    ```

  - 람다식 : 3가지 모두 동일, 원래 메소드의 리턴타입과 메소드명 삭제

    ```java
    (int a, int b) -> {return a > b ? a : b; }
    (int a, int b) -> a > b ? a : b;
    (a, b) -> a > b ? a : b;
    ```

- 함수형 인터페이스

  - 한 개의 추상메소드를 가진 인터페이스를 통해 람다식 구현

  - 함수형 인터페이스 코드

    - 함수형 인터페이스

      ```java
      @FunctionalInterface //함수형 인터페이스 : 구현해야할 추상 메소드가 한 개임을 명시
      public interface LambdaInterface {
      	int max (int a, int b);
      //	int max2 (int a, int b); 사용 불가 : 추상메소드가 하나만 존재해야한다 
      	
      	default int max2 (int a, int b) {// default method는 별개
      		return a > b ? a : b;
      	}
      	static int max3 (int a, int b) {// static method는 별개
      		return a > b ? a : b;
      	}
      }
      ```

    - 함수형 인터페이스 테스트

      ```java
      package lambda;
      
      import java.util.ArrayList;
      import java.util.Collections;
      import java.util.Comparator;
      import java.util.List;
      
      public class LambdaInterfaceTest {
      	public static void main(String[] args) {
      		// 함수형 인터페이스 구현
      		//1. 익명 클래스로 구현
      		LambdaInterface lambdaInterface1 = new LambdaInterface() {
      			@Override
      			public int max(int a, int b) {
      				return a > b ? a : b;
      			}
      		};
      		
      		//2. 람다식 이용 -> 익명클래스의 객체와 동일
      		LambdaInterface lambdaInterface2 = (int a, int b) -> { return a > b ? a : b; };
      		
      		//3. 람다식 이용 -> 타입 생략 가능
      		LambdaInterface lambdaInterface3 = (a, b) -> a > b ? a : b; 
      		
      		System.out.println(lambdaInterface1.getClass());
      		System.out.println(lambdaInterface2.getClass());
      		System.out.println(lambdaInterface3.getClass());
      		
      		//List 정렬 -> Comparator 사용
      		List<Integer> list = new ArrayList<>();
      		for (int i = 1; i <= 5; i++) {
      			list.add(i);
      		}
      		
      		//내림차순정렬
      		Collections.sort(list, new Comparator<Integer>() {
      			@Override
      			public int compare(Integer o1, Integer o2) {
      				return o2 - o1;
      			}
      		});
      		//람다식으로 표현
      		Collections.sort(list, (o1, o2) -> o2-o1);
      		
      		System.out.println(list);
      		
      		//사용자 정의 객체(필드가 2개 이상) 정렬 -> Comparator사용
      		List<Pair> list2 = new ArrayList<>();
      		list2.add(new Pair(1, "강호동"));
      		list2.add(new Pair(2, "유재석"));
      		list2.add(new Pair(3, "유재석"));
      		list2.add(new Pair(4, "강호동"));
      		
      		// 이름 오름차순, 이름이 같다면 id 순
      		Collections.sort(list2, new Comparator<Pair>() {
      			@Override
      			public int compare(Pair o1, Pair o2) {
      				if (!o1.name.equals(o2.name)) {
      					return o1.name.compareTo(o2.name);
      				} else {
      					return o1.id - o2.id;
      				}
      			}
      		});
      		
      		//람다식으로 표현
      		Collections.sort(list2, (o1, o2) -> {
      			if(!o1.name.equals(o2.name)) {
      				return o1.name.compareTo(o2.name);
      			} else {
      				return o1.id - o2.id;
      			}
      		});
      		
      		System.out.println(list2);
      	}
      	
      	static class Pair {
      		int id;
      		String name;
      
      		public Pair(int id, String name) {
      			this.id = id;
      			this.name = name;
      		}
      
      
      		@Override
      		public String toString() {
      			return "[id=" + id + ", name=" + name + "]";
      		}
      	}
      	
      }
      ```

      실행 결과

      ```
      class lambda.LambdaInterfaceTest$1
      class lambda.LambdaInterfaceTest$$Lambda$1/0x0000000800060840
      class lambda.LambdaInterfaceTest$$Lambda$2/0x0000000800061040
      [5, 4, 3, 2, 1]
      [[id=1, name=강호동], [id=4, name=강호동], [id=2, name=유재석], [id=3, name=유재석]]
      ```


- java.util.function패키지

  - 일반적으로는 함수형 인터페이스를 직접 만들기보단 function패키지에서 제공하는 인터페이스를 사용한다
  - 기본 인터페이스(매개 변수가 없거나 하나)
    - `java.lang.runnable`
      - 메소드 : `void run()`
      - 특징 : 매개변수X, 리턴X
    - `Supplier<T>`
      - 메소드: `T get()`
      - 특징 : 매개변수X, 리턴O (`T`)
    - `Consumer<T>`
      - 메소드 : `void accept(T t)`
      - 특징 : 매개변수O (`T`), 리턴X
    - `Function<T,R>`
      - 메소드 : `R apply(T t)`
      - 특징 : 매개변수O (`T`), 리턴O (`R`)
      - 변형
        - `UnaryOperator<T>`
    - `Predicate<T>`
      - 메소드 : `boolean test(T t)`
      - 특징 : 매개변수O (`T`), 리턴O (`boolean`)
  - 매개변수가 2개
    - `BiConsumer<T,U>`
    - `BiFunction<T,U,R>` -> 변형 : `BinaryOperator<T>`
    - `BiPredicate<T,U>`

  - 3개 이상? -> 함수형 인터페이스로 직접 정의

  - 테스트1 - 컬렉션 프레임워크에 있는 함수형 인터페이스 적용

    ```java
    import java.util.*;
    
    public class FunctionTest1 {
    	public static void main(String[] args) {
    		List<Integer> list = new ArrayList<>();
    		for (int i = 0; i < 10; i++) {
    			list.add(i);
    		}
    		
    		list.forEach((i) -> {System.out.print(i+" "); });// Consumer
    		System.out.println();
    		
    		list.removeIf((x) -> x % 2 == 0 || x % 3 ==0);// Predicate
    		System.out.println(list);
    		
    		list.replaceAll((i) -> i*10); //UnaryOperator
    		System.out.println(list);
    		
    		Map<String, String> map = new HashMap<>();
    		map.put("1", "1");
    		map.put("2", "2");
    		map.put("3", "3");
    		map.put("4", "4");
    		
    		map.forEach((k, v) -> {System.out.print("("+k+", "+v+"), "); });// BiConsumer
    	}
    }
    ```

    실행결과

    ```
    0 1 2 3 4 5 6 7 8 9 
    [1, 5, 7]
    [10, 50, 70]
    (1, 1), (2, 2), (3, 3), (4, 4), 
    ```

  - 테스트2 - function패키지의 함수형 인터페이스 활용

    ```java
    import java.util.*;
    import java.util.function.*;
    
    public class FunctionTest2 {
    	public static void main(String[] args) {
    		Supplier<Integer> s = () -> (int) (Math.random()*100)+1;
    		Consumer<Integer> c = i -> {System.out.print(i+", "); };
    		Predicate<Integer> p = i -> i%2==0;
    		Function<Integer, Integer> f = i -> i/10*10;// 일의 자리를 없애기
    		
    		List<Integer> list = new ArrayList<>();
    		makeRandomList(s, list);
    		System.out.println(list);
    		printEvenNum(p, c, list);
    		System.out.println(doSomething(f, list));
    	}
    	
    	
    	private static <T> List<T> doSomething(Function<T, T> f, List<T> list) {
    		List<T> newList = new ArrayList<>(list.size());
    		
    		for (T t : list) {
    			newList.add(f.apply(t));
    		}
    		return newList;
    	}
    
    
    	private static <T> void printEvenNum(Predicate<T> p, Consumer<T> c, List<T> list) {
    		System.out.print("[");
    		for (T t : list) {
    			if (p.test(t)) {
    				c.accept(t);
    			}
    		}
    		System.out.println("]");
    	}
    
    
    	static <T> void makeRandomList(Supplier<T> s, List<T> list) {
    		for (int i = 0; i < 10; i++) {
    			list.add(s.get());
    		}
    	}
    }
    ```

    실행결과

    ```
    [43, 5, 35, 40, 86, 60, 21, 45, 6, 20]
    [40, 86, 60, 6, 20, ]
    [40, 0, 30, 40, 80, 60, 20, 40, 0, 20]
    ```

- Function과 Predicate의 결합

  - Function : 수학에서의 함수처럼 결합(합성)이 가능

    - g(f(x)) 를 표현하려면 
    - `f.andThen(Function g)`
    - `g.compose(Function f)`

  - Predicate : boolean연산(!(not), and, or) 가능

    - !(not) : `negate()`
    - or : `or(Predicate p)`
    - and : `and(Predicate p)`

  - 테스트

    ```java
    import java.util.function.Function;
    import java.util.function.Predicate;
    
    // 16진수 -> 2진수
    public class FunctionTest3 {
    	public static void main(String[] args) {
    		Function<String, Integer> f = (s) -> Integer.parseInt(s, 16);
    		Function<Integer, String> g = (i) -> Integer.toBinaryString(i);
    		Function<String, String> h1 = f.andThen(g);
    		System.out.println(h1.apply("FF"));
    		
    		Function<String, String> h2 = g.compose(f);
    		System.out.println(h2.apply("FF"));
    		
    		Predicate<Integer> p = i -> i < 100;
    		Predicate<Integer> q = i -> i < 200;
    		Predicate<Integer> r = i -> i%2 == 0;
    		Predicate<Integer> notP = p.negate(); //i >= 100
    		
    		Predicate<Integer> all = notP.and(q.or(r));
    		System.out.println(all.test(150));
    		System.out.println(all.test(311));
    	}
    }
    ```
    
    실행결과
    
    ```
    11111111
    11111111
    true
    false
    ```

- 메소드 참조

  - 하나의 메소드만 호출하는 람다식 -> `클래스명(타입)::메소드명` or `참조변수::메소드명`
  - 간결하지만 어떤 매개변수가 필요한지 바로 보이지 않는다

  - 테스트

    ```java
    import java.util.function.*;
    
    public class FunctionTest4 {
    	public static void main(String[] args) {
    		Function<String, Integer> f1 = s -> Integer.parseInt(s);
    		Function<String, Integer> f2 = Integer::parseInt;
    		System.out.println(f1.apply("23"));
    		System.out.println(f2.apply("23"));
    		
    		BiPredicate<String, String> p1 = (s1, s2) -> s1.equals(s2);
    		BiPredicate<String, String> p2 = String::equals;
    		System.out.println(p1.test("A", "A"));
    		System.out.println(p2.test("A", "A"));
    	}
    }
    ```

    실행 결과

    ```
    23
    23
    true
    true
    ```

<br>
