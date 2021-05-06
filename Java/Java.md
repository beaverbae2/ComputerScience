##  Java

- [JVM](#JVM)
- [GC의 동작 원리](#GC의-동작-원리)
- [객체 지향 프로그래밍](#객체-지향-프로그래밍)
- [객체 지향의 4가지 특징](#객체-지향의-4가지-특징)
- [final static abstract](#final-static-abstract)
- [추상클래스와 인터페이스](#추상클래스와-인터페이스)
- [불변 객체](#불변-객체)
- [예외처리](#예외처리)
- [문자열 처리](#문자열-처리)
- [직렬화](#직렬화)
- [Comparable과 Comparator](#Comparable과-Comparator)

<br>

### JVM

**참고 자료**

- [JVM](https://asfirstalways.tistory.com/158)

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
      - java9 부터 default GC

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

<br>

### 불변 객체

- 참고자료

  - [conatuseus.log - [Java] Immuatable Objec(불변객체)](https://velog.io/@conatuseus/Java-Immutable-Object%EB%B6%88%EB%B3%80%EA%B0%9D%EC%B2%B4)
  - [자주와조요 - 불변 객체란?](https://velog.io/@kyle/%EB%B6%88%EB%B3%80-%EA%B0%9D%EC%B2%B4%EB%9E%80-Java-Immutable-Object)

- 불변 객체란

  - 한 번 생성 후, 외부에서 변경 불가능한 객체

- 장단점

  - 장점(사용목적)
    - 객체 신뢰도 증가 : transaction 할 때 유용
    - 생성자 및 접근 메소드에 대해 복사 방어 필요 없음
    - 멀티스레드 환경에서 동기화 처리없이 객체 공유
  - 단점
    - 매번 새로운 객체가 필요 -> 메모리 누수 및 성능저하 발생

- 구현

  - 필드가 primitive type만 있는 경우 : setter사용X (필수), final 키워드 사용(선택)

    ```java
    class PrimitiveObject {
    	final int value;
    
    	public PrimitiveObject(int value) {
    		this.value = value;
    	}
    
    	public int getValue() {
    		return value;
    	}
    }
    ```

  - 필드에 reference type이 있는 경우

    - 단일 객체만 있는 경우 : 해당 객체도 불변 객체로 생성

      ```java
      // 1. 필드에 단일 객체가 있는 경우
      class ReferenceObject1 {
      	private final Pair p;
      
      	public ReferenceObject1(Pair p) {
      		this.p = p;
      	}
      
      	public Pair getP() {
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

      - 생성 : 인자로 들어오는 배열의 요소를 가지는 **새로운 배열 객체 생성**
      - getter : 외부에서 수정 불가능하게 **원본과 다른 복사본을 생성**해서 리턴

      ```java
      //2. 필드에 배열이 있는 경우
      class ReferenceObject2 {
      	private final Pair[] arr;
      	
      	public ReferenceObject2(Pair[] arr) {
      //		this.arr = arr; // 필드arr이 인자arr 주소를 참조 -> 인자arr의 값이 변경되면 필드arr의 값도 변경(not immutable) 
      		this.arr = Arrays.copyOf(arr, arr.length); // 인자 arr의 요소를 갖는0 새로운 객체 생성 -> 필드arr와 인자arr의 주소가 다름 
      	}
      
      	public Pair[] getArr() {
      //		return arr;// 외부에서 변경 가능 
      		return (arr == null) ? null : arr.clone();// 복사본 리턴 : 외부에서 변경해도 영향 없음
      	}
      }
      ```

    - List가 있는 경우

      - 생성 : 인자로 들어오는 List의 요소를 가지는 **새로운 List 객체 생성**
      - getter : 외부에서 수정 불가능하게 `Collections.unmodifiableList(List list);` 사용

      ```java
      //3. 필드에 리스트가 있는 경우
      class ReferenceObject3 {
      	private final List<Pair> list;
      
      	public ReferenceObject3(List<Pair> list) {
      //		this.list = list; // 필드list가 인자list 주소를 참조 -> 인자list의 값이 변경되면 필드list의 값도 변경(not immutable) 
      		this.list = new ArrayList<>(list);// 인자 list의 요소로 갖는 새로운 객체 생성 -> 필드list와 인자list의 주소가 다름 
      	}
      
      	public List<Pair> getList() {
      //		return list; // 외부에서 변경 가능
      		return Collections.unmodifiableList(list);// 외부에서 변경 시도시 UnsupportedOperationException 발생
      	}
      }
      ```

- 참고
  
- final을 사용해도 일반적인 getter를 사용하면 배열이나 list 리턴 받고 수정 가능하다 -> 주소가 동일하기 때문
  
- 요약

  - 기본: setter X, final 사용

  - primitive type
    - 기본대로
  - referrence type
    - 단일 객체 : 참조하는 객체도 기본대로 불변객체로 생성
    - 배열, 리스트
      - 주소를 다르게 한다
      - 생성시 인자로 들어오는 요소들을 똑같이 가지는 새로운 객체 생성
      - getter 사용시 외부에서 변경 불가능하게 처리
        - 배열 : 복사본을 따로 만들어서 보냄 (수정해도 원본에 영향X)
        - List : `Collections.unmodifiableList(List list);` 로 변경 원천 차단

<br>

### 예외처리

- 참고 자료
  
- [기적을 만드는 기록 - 예외 & 예외 처리란?](https://jiwontip.tistory.com/5)
  
- 에러와 예외
  - 에러 : 수습 불가능한 오류
  - 예외 : 수습가능한 다소 미약한 오류
- 예외 클래스의 계층 구조
  -  Throwable
    - Exception
      - (Checked Exception) - compile 시 확인
        - IOException
        - ....
        - ClassNotFoundException
      - (Unchecked Exception) RuntimeException - runtime 확인
        - ArithmeticException
        - NullPointerException
        - ...
        - IndexOutOfBoundException

- 예외 처리 방법

  - try-catch-finally

    ```java
    try {
        // 예외를 처리하길 원하는 실행 코드 (예외가 발생할 수 있는 코드)
    } catch (e1) {
        // e1 예외가 발생시 실행되는 코드
    } catch (e2) {
        // e2 예외가 발생시 실행되는 코드
    } finally {
        // 예외 발생 여부와 관계 없이 실행 
    }
    ```

  - throw, throws

    - throw : 강제로 예외 발생 시키기
    - throws : 메소드를 호출한 상위 메소드에서 예외를 처리

    - 사용자 정의 예외 클래스 : Exception이나 RuntimeException 클래스를 상속받아 구현 

    - 구현 예시

      ```java
      // 사용자 정의 스택 구현
      public class Test {
      	public static void main(String[] args) {
      		MyStack stack = new MyStack(3);
      		stack.push(1);
      		stack.push(2);
      		System.out.println(stack.pop());
      		stack.push(3);
      		stack.push(4);
      		stack.push(5);
      		System.out.println(stack.pop());
      		System.out.println(stack.pop());
      		System.out.println(stack.pop());
      		System.out.println(stack.pop());
      		System.out.println(stack.pop());
      	}
      }
      
      // 배열로 stack구현
      class MyStack {
      	int[] arr;
      	int size;
      	int top;
      	
      	{
      		top = 0;
      	}
      	
      	public MyStack(int size) {
      		this.arr = new int[size];
      		this.size = size; 
      	}
      	
      	public void push(int data) throws FullException {// 이를 호출한 상위 메소드에서 예외 처리
      		if (isFull()) {
      			throw new FullException("stack is full!");// 예외발생 시키기
      		} else {
      			arr[top++] = data;
      		}
      	}
      	
      	public int pop() throws EmptyException {// 이를 호출한 상위 메소드에서 예외 처리
      		if (isEmpty()) {
      			throw new EmptyException("stack is empty!");// 예외발생 시키기
      		} else {
      			top--;
      			return arr[top];
      		}
      	}
      	
      	public boolean isEmpty() {
      		if (top == 0) return true;
      		return false;
      	}
      	
      	public boolean isFull() {
      		if (top == size) return true;
      		return false;
      	}
      }
      
      // 사용자 정의 예외 클래스 구현
      class EmptyException extends RuntimeException{
      	/**
      	 * add this because Serializable
      	 */
      	private static final long serialVersionUID = 1L;
      
      	public EmptyException() {
      		super();
      	}
      	
      	public EmptyException(String msg) {
      		super(msg);
      	}
      }
      
      // 사용자 정의 예외 클래스 구현
      class FullException extends RuntimeException{
      	/**
      	 * add this because Serializable
      	 */
      	private static final long serialVersionUID = 1L;
      
      	public FullException() {
      		super();
      	}
      	
      	public FullException(String msg) {
      		super(msg);
      	}
      }
      
      ```

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
  - Java의 정석 직렬화Part (p.934~p.944)
- 직렬화, 역직렬화
  - 직렬화 : 객체나 데이터를 byte형태로 변환
  - 역직렬화 : byte형태의 데이터를 객체나 데이터로 변환
  - 객체를 저장했다가 다시 꺼내쓸 때, 또는 서로 간의 객체를 주고 받기 위해 직렬화 사용

- 구현

  - 직렬화

    - 조건 - `java.io.Serializable` 상속

      ```java
      class UserInfo implements Serializable {
      	String name;
      	transient String password;
      	int age;
      	
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

        - 직렬화와 역직렬화를 진행하는 시스템이 서로 다를 수 있음을 인지

        - 동일한 `serialVersionUID`를 가지고 있어야 함

          - `serialVersionUID`는 클래스가 변경(메소드 or 필드 변경) 될 때마다 변경
          - 그렇기에 일반적으로 클래스의 필드에 `serialVersionUID` 명시 동일한 값을 갖게 함
          - `serialVersionUID`가 다를 경우 `java.io.InvalidClassException` 발생
          - `serialVersionUID` 가 같아도 변수의 type이 달리진 경우 역직렬화 불가

        - 직렬화할 때와 역직렬화 할 때의 순서는 동일해야 함

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
              private static final long serialVersionUID = 1L;//SUID 명시
            
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

<br>

### Comparable과 Comparator

- 객체의 정렬을 위해서 사용
  - comparable : 기본 정렬기준을 구현 (클래스 내에서 구현) -> `Colletions.sort(list)`
  - comparator : 기본 정렬기준 외에 다른 기준으로 정렬 -> `Collections.sort(list, ComparatorObj)`

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

### 익명 클래스 (Anonymous Class)

- 이름이 없는 클래스

  - 어떤 클래스를 상속하거나 인터페이스를 구현하여 **일회성으로 사용**

  - getClass()로 어떤 클래스인지 확인할때 클래스명이 나오지 않음

- 익명 클래스

  - 코드

    ```java
    package anomymous;
    
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
    package anomymous;
    
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

    

