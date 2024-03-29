# Spring MVC1 (김영한님 강의)

## 웹 서버와 WAS(Web Application Server)

### 웹 서버

- HTTP 기반으로 동작
- **정적 리소스** 제공(HTML, CSS, JS, 이미지,....)
- ex) nginx, apache, ...

### WAS

- HTTP 기반 으로 동작
- 웹서버 기능 포함(정적 리소스 제공)
- 프로그램 코드를 실행해서 **어플리케이션 로직 수행**
  - 동적 HTML, HTTP API(JSON)
  - 서블릿, JSP, 스프링 MVC
- ex) tomcat, jetty, ....

### 그래서 차이는?

WAS는 어플리케이션 코드를 실행 하는데 특화

### 웹 시스템 구성

#### WAS, DB

**문제점**

- WAS가 너무 많은 역할 담당 -> **과부하**
- 가장 **비싼** 어플리케이션 로직이 정적 리소스 때문에 수행이 어려울 수 있음
- WAS 장애(장애 잘 난다...)시 오류 화면도 노출 불가능

#### 웹 서버, WAS, DB

- 정적 리소스는 웹 서버가 처리
- 웹 서버는 어플리케이션 로직 같은 동적 처리는 WAS에 위임
- WAS는 어플리케이션 로직에만 집중
- 효율적인 리소스 관리
  - 정적 리소스가 많이 사용 되면 -> 웹 서버 증설
  - 어플리케이션 리소스가 많이 사용 되면 -> WAS 증설
- WAS, DB 장애서 웹 서버가 오류 화면 제공 가능
  - 웹 서버는 잘 안죽음 -> 정적 리소스 제공만 해주면 되므로
  - WAS는 잘 죽음 -> 어플리케이션 로직이 동작하므로

### 요약

- 웹 서버는 정적 리소스 제공 -> 잘 안죽음
- WAS는 웹 서버의 역할과 어플리케이션 로직 수행 가능 -> 잘 죽음
- 웹 시스템은 일반적으로 웹 서버, WAS, DB로 구성
  - WAS는 어플리케이션 로직만 담당해서 부하 감소
  - 상황에 맞게 웹 서버와 WAS 증설 가능
  - WAS와 DB 장애 발생 시 오류 화면 제공 가능

<br>

## 서블릿(Servlet)

### 서블릿이란

- 개발자가 HTTP 스펙을 매우 편리하게 사용 -> 개발자가 비즈니스 로직에만 집중 가능
- urlPatterns의 URL이 호출되면 서블릿 코드 실행
- 요청 : HttpServletRequest 사용
- 응답 : HttpServletResponse 사용

### HTTP 요청, 응답 흐름

- HTTP 요청 시
  - WAS는 Request, Response 객체를 새로 생성 한 후, 서블릿 객체 호출
  - 개발자는 Request 객체에서 HTTP 요청 정보를 편리하게 꺼내서 사용
  - 개발자는 Response 객체에 HTTP 응답 정보를 편리하게 입력
  - WAS는 Response 객체에 담겨있는 내용으로 HTTP 응답 정보 생성

### 서블릿 컨테이너

- 서블릿을 지원하는 WAS
- 서블릿 생명주기 관리 -> 생성, 초기화, 호출, 종료
- **서블릿 객체는 싱글톤으로 관리**
  - 효율성 높이기 위함 
    - 매 요청 마다 서블릿 객체 생성하는 것은 비효율적
    - 최초 로딩 시점에 서블릿 객체를 미리 만들어 놓고 재활용
    - 모든 요청은 동일한 서블릿 객체에 접근
  - 주의 : 공유 변수 사용 -> 임계 영역 문제
  - 서블릿 컨테이너 종료 시 함께 종료
- JSP도 서블릿으로 변환 되서 사용
- **동시 요청을 위한 멀티 쓰레드 처리 지원**

### 요약

- 서블릿을 통해 개발자는 비즈니스 로직에 집중 가능 -> 반복되는 HTTP 설정 작업에서 해방
- 구조
  - HttpServletRequest 객체 : 개발자가 요청 정보를 편리하게 가져올 수 있음
  - HttpServletResponse 객체 : 개발자가 응당 정보를 편리하게 담을 수 있음
- 서블릿 컨테이너는 서블릿을 관리
- 서블릿은 싱글톤으로 관리, 동시 요청을 위한 멀티쓰레드 처리 지원

<br>

## 동시 요청 - 멀티 쓰레드 (중요)

### 쓰레드

- 어플리케이션 코드를 하나하나 순차적으로 실행
- 동시 처리가 필요하면 쓰레드 추가 생성
- 요청이 발생할 때마다 커넥션이 생생되면서  쓰레드도 생성, 쓰레드가 servlet 객체 호출

### 다중 요청

#### 쓰레드 하나 사용

- 앞의 request가 처리 지연되는 경우, 뒤의 request의 처리가 매우 늦어짐

#### 요청 마다 쓰레드 생성

- 장점
  - 동시 요청 처리
  - 리소스(CPU, 메모리)가 허용할 때 까지 처리가능
  - 하나의 쓰레드가 지연되어도, 나머지 쓰레드 정상 동작

- 단점
  - 비싼 생성 비용-> 고객 요청이 올 때마다 쓰레드 생성하면 응답 속도 지연
  - 컨텍스트 스위칭 비용 발생
  - 생성에 제한이 없음 -> 요청이 너무 많이 오면 CPU나 메모리 임계점을 넘는 경우 서버 다운

#### 쓰레드 풀

- 요청 마다 쓰레드 생성하는 단점을 보완
- 특징
  - **필요한 쓰레드를 쓰레드 풀에 보관하고 관리**
  - 쓰레드 풀에 생성 가능한 쓰레드의 최대치 관리. 톰캣은 최대 200개가 기본 설정(변경 가능)
- 사용
  - 쓰레드가 필요하면(요청이 발생하면), 쓰레드 풀에 이미 생성 되어 있는 쓰레드를 꺼내서 사용
  - 사용 종료(응답) 후, 쓰레드 풀에 해당 쓰레드 반납
  - 최대 갯수의 쓰레드가 모두 사용 중이라, 쓰레드 풀에 쓰레드가 없는 경우
    - 요철을 거절하거나, 특정 개수 대기
- 장점
  - 쓰레드가 미리 생성되어 있으므로, 쓰레드를 생성하고 종료하는 비용(컨텍스트 스위칭 비용, CPU) 절약, 빠른 응답 시간
  - 너무 많은 요청이 들어와도 기존 요청을 안전하게 절약 가능 -> 생성 가능한 쓰레드의 최대치가 정해져 있으므로

- 실무 팁
  - WAS의 주요 튜닝 포인트는 최대 쓰레드(max thread)의 수
    - 너무 낮은 경우 : 동시 요청 증가시, 서버 리소스는 여유롭지만, 클라이언트는 응답 지연 발생
    - 너무 높은 경우 : 동시 요청 많으면, CPU와 메모리 리소스 임계점 초과로 서버 다운
  - 장애 발생 시?
    - 클라우드 : 서버 늘리고 튜닝
    - 아니면 : 열심히 튜닝
  - 그렇다면 적절한 최대 쓰레드 숫자를 찾는 방법?
    - 변수 : 어플리케이션 로직의 복잡도, CPU, 메모리, IO 리소스 상황에 따라 다름
    - 성능 테스트
      - 최대한 실제 서비스와 유사하게 성능 테스트 시도
      - 툴 : 아파치 ab, 제이미터, **nGrinder**

#### 핵심

- 개발자가 멀티 쓰레드 관련 코드를 신경 쓰지 않아도 됨
  - **멀티 쓰레드에 대한 부분은 WAS가 처리**
  - 개발자는 싱글 쓰레드 프로그래밍을 하듯이 편리하게 코딩
- 주의 : 멀티 쓰레드 환경이므로, 싱글톤 객체(서블릿, 스프링 빈)는 주의해서 사용 -> 공유 변수 문제

<br>

## HTML, HTTP API, CSR, SSR

### HTTP를 통해 데이터를 주고 받는 3가지 방법

#### 1. 정적 리소스

- 고정된(이미 생성된) HTML 파일, CSS, JS, 이미지, 영상 등을 제공
- 주로 웹 브라우저

#### 2. HTML 페이지

- 동적으로 필요한 HTML 파일을 생성해서 전달
- 웹 브라우저 : HTML 해석
- JSP, 타임리프 등을 사용

#### 3. HTTP API

- HTML이 아니라 **데이터**를 전달
  - 주로 JSON 형식 사용
- **다양한 시스템(플랫폼)에서 호출 가능**
  - 데이터만 주고 받음, UI가 필요하면, 클라이언트 측에서 별도 처리
  - UI 클라이언트 접점
    - 앱 클라이언트 -> 아이폰, 안드로이드, PC앱
    - 웹 브라우저에서 JS를 통해 HTTP API 호출
    - React, Vue.js 같은 웹 클라이언트
  - 서버 to 서버
    - ex) 주문 서버 -> 결제 서버
    - 기업간 데이터 통신

### 렌더링

#### 1. SSR - Server Side Rendering

- **서버**에서 최종 HTML을 생성해서 클라이언트에 전달
- 주로 정적인 화면에 활용
- 관련 기술 : JSP, 타임리프 -> **백엔드 개발자**

#### 2. CSR - Client Side Rendering

- HTML 결과를 JS를 사용해 **웹 브라우저**에서 동적으로 생성
- 웹 환경을 마치 앱처럼 필요한 부분만 변경 가능
- 관련 기술 : React, Vue.js -> **웹 프론트엔드 개발자**
- 과정
  - 서버에 HTML 요청 : 서버가 HTML 내용은 없고, JS 링크 응답
  - 서버에 자바스크립트 요청 : 서버가 JS 클라이언트 로직, HTML 렌더링 코드 응답
  - HTTP API(데이터 요청) : 서버가 데이터 응답
  - 웹 브라우저에서 JS로 HTML 결과 렌더링

#### 참고

- React, Vue.js를 CSR + SSR 동시에 지원하는 웹 프레임워크도 있음
- SSR을 사용해도, JS를 통해 필요한 화면 동적으로 변경 가능

<br>
