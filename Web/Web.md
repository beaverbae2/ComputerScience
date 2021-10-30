# Web

### 목차

- [CORS](#CORS)

<br>



### CORS

#### SOP(Same Origin Policy)

> 동일 출처 정책,  다른 출처의 리소스를 사용하는 것을 제한 -> 보안

**Origin**

다음과 같은 URL이 있다고 하자

`https://beaver.com:443/course?name=web`

- URL의 `protocol`, `host`, `port`를 통해 같은 origin인지 판단
- 위 예시의 경우, origin은 `https://beaver.com:443` 

-> **SOP로 인해 다른 출처의 리소스 사용하는 것 불가**

#### CORS(Cross Origin Resource Sharing)

> 교차 출처 리소스 공유
>
> 추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 어플레케이션이 **다른 출처의 리소스에 접근**할 수 있는 권한을 부여하도록 **브라우저**에 알려주는 체제

**CORS 접근 제어 종류**

- Preflight Request

  - 요약

    ![cors preflight](https://evan-moon.github.io/static/c86699252752391939dc68f8f9a860bf/6af66/cors-preflight.png)

    - 2개의 요청을 보냄
    - 먼저, `preflight request`를 보내어 다른 도메인의 리소스에 요청이 가능한 지 확인 
    - 요청이 가능하다면, 실제 요청(`main request`)를 보낸다

  - 상세 과정

    ![Cross-Origin Resource Sharing](https://media.vlpt.us/images/fkqb90/post/1e7852d0-2202-4019-a347-e215a04a2e09/preflight_correct.png)

    - preflight request
      - Method : `OPTIONS`
      - Origin : 요청 출처
      - Access-Control-Request-Method : 실제 요청의 메소드
      - Access-Control-Request-Header : 실제 요청의 추가 헤더
    - preflight response
      - Access-Control-Origin : 서버 측 허가 출처(`*` : 모든 출처 허가)
      - Access-Control-Allow-Method : 서버 측 허가 메소드
      - Access-Control-Allow-Header : 서버 측 허가 헤더
      - Access-Control-Allow-Max-Age : 응답 캐시 기간, 캐시 기간 동안은 실제 요청만 보냄

  - 요청을 굳이 두번 보내는 이유
    - **서버는 CORS를 모름**, 이미 작업 다 해서 전달 했는데 **브라우저**에서 CORS 차단
    - preflight가 없다면? 
      - 예를 들어, 클라이언트 측에서 `DELETE` 요청을 보내면
      - 서버 측은 DB의 데이터를 이미 삭제하고 응답을 함
      - 하지만 브라우저는 리소스의 출처가 다르므로 CORS 문제 발생 시킴
      - 즉, 서버는 애꿎은 데이터만 삭제함

- Simple Request

  - preflight 없이 바로 실제 요청 날림

  - 요청 조건

    - Method : `GET`, `POST`, `HEAD` (셋 중 하나만 허용)
    - Content-Type : `application/x-www-form-urlenencoded`, `multipart/form-data`, `text/plain`
    - Header : `Accept`, `Accept-Language`, `Content-Language`, `Content-Type` 만 허용

  -  과정

    ![CORS &amp; SOP에 대해 알아보자 -2탄](https://media.vlpt.us/images/sj950902/post/5749e233-2da6-4b79-82b8-8b244a72d7de/cors_simplerequest.png)

    - 만약 서버의 Access-Control-Allow-Origin에 요청 출처가 없다면 브라우저에서 CORS 에러 발생시킴

- Credential Request

  - 인증 관련(ex)jwt, ...) 헤더를 포함할 때 사용
  - 클라이언트 측
    - `credentials` : `include`
  - 서버 측
    - Access-Control-Allow-Credential : true
    - 단, Access-Control-Allow-Origin : * 은 허용하지 않음

#### CORS 해결방법

- 프론트 프록시 서버

- 헤더 직접 설정

- 스프링 부트 활용

  - `@CrossOrigin` 

    - 현재 컨트롤러에 오는 요청은 CORS를 허용한다는 것 명시
    -  `origin` 옵션을 통해 출처 명시 가능

  - Configuration 활용 -> `WebMvcConFigurer` 상속

    - 특정 컨트롤러가 아닌, 서버로 오는 모든 요청에 대한 CORS를 허용하는 것 명시

    -  `WebMvcConFigurer` 상속 후, `addCorsMapping()` 메소드를 오버라이딩하여 적용

      