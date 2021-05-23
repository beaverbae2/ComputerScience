# Network

- [URI와 URL](#uri와-url)

- [GET과 POST](#get과-post)

<br>

### URI와 URL

- URI 
  - Uniform Resource Identifier
  - 네트워크 상의 **자원 식별자**
- URL
  - Uniform Resource Locator
  - 인터넷에 있는 자원을 나타내는 **유일한 주소(위치)**
- URI가  URL의 상위 개념
- 비교
  - www.example.com : URI, URL
  - www.example.com/result.html : URI, URL
  - www.example.com/result.html?id=123 : URI
    - 식별자(Identifier. 여기서 id값)에 따라 결과가 달라짐

<br>

### HTTP 헤더

- https://wonit.tistory.com/308



### GET과 POST

- 참고자료
  - [기록하기 - GET과 POST의 차이](https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/)
  - [JaeYeopHan - GET과 POST](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network#http%EC%9D%98-get%EA%B3%BC-post-%EB%B9%84%EA%B5%90)

- **요약 : GET은 데이터 조회, POST는 데이터 생성/변경**

- GET 

  - 주로 자원을 **조회**를 할 때 사용
  
    - Idempotent(멱등) 
      - 동일한 연산에 대해 같은 결과가 나옴
    - 즉, 서버에 동일한 요청을 여러번 하면 응답 동일

  - 동작 방식

    > www.example-url.com/resources?name1=value1&name2=value2

    -> 의미 : www.example-url.com/resource 라는 URI에서 name1 = value1이고 name2 = value2 이라는 자원을 보여주세요
  
    - **쿼리 스트링**
      - ? 뒤에 붙는것
        - ? 뒤에 파라미터=값 형태로 붙음
        - & : 파라미터가 여러개 있는 경우
      - 요청 파라미터 : name1, name2
      - 파라미터의 값
        - name1 : value1
        - name2 : value2
  
  - 특징
    - `HTTP Request Message`의 **Header의 URL 에 담겨서 전송**
    - 파라미터와 값이 URL에 그대로 노출(보안에 취약)
    - 응답 데이터가 브라우저의 캐시에 저장

<br>

- POST
  - 자원을 **생성/변경** 하기 위해 사용
    - Non-Idempotent 
      - 동일한 연산에 대해 결과가 다를 수 있음

    - 즉, 서버에 동일한 요청에 대해 응답이 다름

  - 특징
    - `HTTP Request Message`의 **Body에 데이터를 담아 전송**
    - `Content-Type`을 정해줘야함
    - 데이터가 눈에 보이지 않음(그래도 개발자 도구로 내용 확인 가능함... 필요한 경우 암호화 진행)


<br>

### HTTP와 HTTPS

**참고자료**

- [생활코딩 - HTTPS와 SSL 인증서](https://opentutorials.org/course/228/4894)

- [망나니 개발자 - [Web] HTTP와 HTTPS 및 차이점](https://mangkyu.tistory.com/98)

- [한재엽님 블로그](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network)

<br>

**HTTP의 문제점** 

- 암호화되지 않은 평문 데이터를 보낸다 -> 도청 가능 
- 통신 상대 확인 안함 -> 위장 가능
  - 클라이언트 : 의도한 웹 서버에 요청을 잘 보냈는지 확인 불가
  - 서버 : 요청을 보낸 클라이언트에게 올바로 응답하고 있는지 확인 불가
- 완전성(정보의 정확성) 증명 불가(중간자 공격) -> 변조 가능

<br>

**HTTPS 란**

- S : secure -> HTTP 데이터를 암호화

- HTTP 통신하는 소켓 부분을 SSL(Secure Socket Layer)또는 TSL(Transport Socket Layer)로 대체
  - HTTP는 TCP 소켓과 직접 통신
  - HTTPS는 SSL과 통신, SSL이 TCP와 통신
  
- 포트번호
  - HTTP : 80
  - HTTPS : 433
  
- 공개키와 개인키
  - 공개키 : 모두에게 공개
  - 개인키 : 개인만 가지고 있어야되는 키
  - 공개키로 암호화된 데이터는 개인키로 복호화, 개인키로 암호화된 것은 공개키로 복호화
  
- 세션키

  - 클라이언트와 서버가 통신하는 동안 데이터를 암호화, 복호화 하는데 쓰이는 키

- 인증서

  - 클라이언트와 서버의 신원 확인 용도
  - 서버의 공개키가 인증서에 저장

- 인증 기관(CA)

  - 인증서를 발급하는 기관

  - 클라이언트(브라우저)가 가지고 있는 CA 리스트로 부터 확인함
    - 공인 기관 : 안전
    - 사설 기관 : 안전하지 않음

    

- HTTPS 통신 과정 (SSL)

  - 서버가 CA로 부터 인증서 발급
    - 기업은 HTTP 기반의 애플리케이션에 HTTPS를 적용하기 위해 공개키/개인키를 발급
    - CA로 부터 서버의 공개키를 저장하는 인증서 발급 요청
    - CA는 기업명, 서버의 공개키, 서버 정보를 인증서에 저장한 후 CA의 개인키로 암호화
  - Handshake
    - Client Hello
      - 클라이언트(브라우저)가 서버에 접속
      - 전달 내용
        - **클라이언트 측에서 생성한 랜덤 데이터**
        - 지원하는 암호화 방식
        - 세션 아이디
    - Server Hello
      - 서버의 응답
      - 전달 내용
        - **서버 측에서 생성한 랜덤 데이터**
        - 서버가 선택한 암호화 방식
        - **인증서** : CA의 개인키로 암호화 되어 있음
    - 클라이언트의 추가 작업
      - 인증서 확인
        - CA로 부터 만들어진 인증서인가 확인
        - 이미 가지고 있는 CA의 공개키로 인증서 복호화 -> 서버의 공개키 획득
      - pre master key (대칭키) 생성 
        - 클라이언트와 서버측에서 생성한 랜덤데이터의 조합으로 생성
        - 서버의 공개키로 암호화하여 서버로 전송
    - 서버의 추가 작업
      - session key 생성
        - 클라이언트로 부터 받은 pre master key를 서버의 개인키로 복호화
        - 이후 서버와 클라이언트가 추가 작업을 통해 session key 생성하여 공유
  - 세션 (전송)
    - 송신 측은 session키로 데이터를 암호화하여 전송
    - 수신 측은 session키로 데이터를 복호화하여 획득
    - **왜 대칭키와 비대칭키(공개키, 개인키)를 조합하는 방식을 사용하는가?**
      - 비대칭 암호화가 느림
  - 세션 종료
    - 통신 끝 -> session key 폐기

<br>

### Cookie와 Session

**참고자료**

- [로그인은 어떻게 이루어질까?](https://velog.io/@junhok82/%EB%A1%9C%EA%B7%B8%EC%9D%B8%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%9D%B4%EB%A3%A8%EC%96%B4%EC%A7%88%EA%B9%8CCookie-Session)
- [HTTP의 특성](https://victorydntmd.tistory.com/286)

- [쿠키와 세션](https://victorydntmd.tistory.com/34)

<br>

**HTTP의 특성**

- connectionless : 클라이언트의 요청을 서버가 응답하면 연결 끊어짐
- stateless : 상태를 유지하지 않는 성질, 클라이언트와 서버가 독립적(서버가 클라이언트 식별X)

- 이러한 특성으로 인해 서버의 부담을 줄일 수 있음

<br>

**쿠키와 세션**

- 서버측에서 클라이언트를 식별하기 위해 사용

- 쿠키

  - **클라이언트(브라우저)**단에서 관리되는 기록 파일, **HTTP 헤더에 전달된다**

  - 구조

    - 이름 : 식별자(키)
    - 값 
    - 유효시간 
      - 세션 쿠키 : 없으면 브라우저 창 닫으면 파기
      - 영구 쿠키 : 유효시간만큼 살아있음
    - 도메인
    - 요청경로

  - 예시

    - 아이디, 비밀번호 저장
    - n일 간 띄우지 않기 팝업

  - 동작
    
    - 클라이언트 측에서 요청
      - n일간 띄우지 않기 클릭
    - 서버에서 쿠키 생성 후 응답
      - n일간 띄우지 않겠다는 쿠키 생성하여 전달 (유효 기간 : n일)
    - 클라이언트에서 요청시 쿠키를 포함해서 전송
      - n일 동안 팝업 안보게 된다
    
  - 특징
    
    - 유효시간 존재
    - 클라이언트단에 저장되기 때문에 보안에 취약
    
  - 예제

    - 서버로 부터 쿠키 생성

      - 자바 코드

        ```java
        // 키, 값으로 생성
        Cookie nameCookie = new Cookie("name", "tommy");
        Cookie nicknameCookie = new Cookie("nickname", "jerry");
        
        // 유효 시간 설정
        nameCookie.setMaxAge(3600);// 1시간
        ```

      - HTTP 헤더

        ```http
        HTTP/1.1 200 OK
        ...생략
        Set-Cookie: name=tommy
        Set-Cookie: nickname=jerry; Expires=<date>
        ...생략
        ```

    - 클라이언트가 서버로 쿠키 전송

      - 자바 코드

        ```java
        Cookie[] cookies = req.getCookies();// 모든 쿠키 받아오기
        ```

      - HTTP 헤더

        ```http
        HTTP/1.1 200 OK
        ...생략
        Cookie : name=tommy; nickname=jerry
        ...생략
        ```

- 세션

  - **서버**에서 관리 
  - 동작
    - 클라이언트가 서버에 접속하여 세션 생성시 고유한 **세션 ID**(클라이언트 식별) 발급
    - 클라이언트에서 세션 ID를 쿠키로 저장(JSESSIONID)
    - 클라이언트 측에서 서버에 접속시 쿠키로 세션 ID를 서버에 전달 
  - 특징
    - 클라이언트마다 고유 **세션 ID** 부여
    - 클라이언트(브라우저)가 종료되거나 유효시간 만료 될 때 까지 상태 유지
    - 보안 면에서 쿠키보다 우수
    - 사실 세션도 중간에서 탈취 가능하다...
  - 사용법
    - 생성 : `HttpSession session = req.getSession()` , `req = HttpsServletRequest`
      - 쿠키로 받은 JSESSIONID로 해당 세션이 있는지 조회
        - 없다면 세션을 새로 생성한다
    - 값 저장 : `setAttribute("이름", "값")`
    - 값 읽기 : `getAttribute("이름")`
    - 값 삭제 : `removeAttribure("이름")`
    - 유효시간설정 : `setMaxInactiveInterval(시간)`

- 참고) OAuth, JWT 

  - token기반 인증 방식
  - 쿠키와 세션의 문제점 보완

<br>

### Transport Layer(TCP와 UDP)

#### Transport layer와 Network layer의 차이

- TR : process간 통신 -> port -> segment단위로 통신
- Net : host간 통신 -> ip address -> packet단위로 통신

<br>

#### Multiplexing과 Demultiplexing

- process간의 통신

  - application은 여러 process로 이뤄짐

  - 각 process는 socket을 통해 통신

  - port : socket을 구분하는 일종의 구조

  - 한 application은 여러 process를 가지고 있는데 어떻게 서로를 구분해서 통신하는가?

    -> multiplexing, demultiplexing이용

- Multiplexing 
  - 다른 process로 데이터를 올바르게 보내기 위해 하는 작업
  - segment를 구성 하고, segment의 header 적절한 정보 표기
- Demultiplexing
  
  - 받은 segment를 socket에 올바로 보내는 작업
    
  - UDP의 demultiplexing
    - connectionless
    - 확인 요소
      - dest IP
      - dest port
  
  - TCP의 demultiplexing
    - connection-oriented
    - 확인요소
      - src IP
      - src port
      - dest IP
      - dest port

<br>

 #### UDP

- 특징
  - 데이터를 보내는 것에 집중
  - connectionless : 그냥 목적지에 데이터를 보내기만 함
  - unreliable : 데이터가 유실되건 말건 신경 안씀
- 사용
  - DNS
  - streaming multimedia (ex)유튜브)
-  header 구조
  - src port
  - dest port
  - length
  - checksum(에러 확인)

<br>

#### RDT(Reliabe Data Tranfer) - 참고용

> TCP의 reliable한 통신을 위해 간단한 상황을 만들어 TCP의 header나 기능으로 무엇이 필요한지 이해

###### rdt의 기본 전제 

- sender는 패킷을 보내기만 하고 receiver는 받기만함
- 패킷은 **하나씩** 보냄

##### rdt 1.0 

- 완벽함, 할것이 없음

##### rdt 2.0

- 상황 : error발생
- 해결 
  - checksum : 제대로된 패킷인지 확인
  - ack : receiver가 sender에게 잘 받았다고 보냄
  - nak : receiver가 sender에게 못 받았다고 보냄
- header : checksum ack, nak

##### rdt 2.1

- 상황 
  - rdt 2.0에서 문제점 발생
  - ack나 nak에 error가 있다면?
    - sender는 계속 재전송
    - receiver는 패킷의 순서를 파악 못함
- 해결
  - sequence number
    - 패킷의 번호
    - **순서 파악 가능**
    - receiver 측에서 중복된 번호의 패킷은 버리고 ack만 함
- header : checksum, ack, nak, #seq

##### rdt 2.2

- 상황
  - nak가 필요한가 -> 최적화
- 해결
  - receiver에서 응답시 **ack+#seq** 보냄
  - ex) ack 0 : 0번 잘 받았음
- header : checksum, ack, #seq

##### rdt 3.0

- 상황
  - 패킷 loss(유실) 발생 -> 재전송 필요
- 해결
  - timer 설정
    - 효율적으로 설정
    - timeout 되면 sender측에서 패킷 재전송
- header : checksum, ack, #seq
- 기능 : timer

<br>

#### TCP

- 특징

  - connection-oriented : 1대1로 통신
  - reliable : 데이터가 유실 안되게함
  - in-order : 순서 존재
  - bi-directional

- header 구조

  - src port
  - dest port
  - seq number(번호), ack number(잘 받았고 다음에 x번 segment 줘) -> in-order

  - flag (SYN, FIN)
  - ....


<br>

#### TCP : reliable transfer

> 신뢰할 수 없는 채널을 가진 상황에서, TCP가 신뢰성 있는 전송을 함

-  sequence number와 ACK
    - segment를 순서대로 보내기 위해 사용
    - #seq : segment의 첫번째 바이트 값
    - ACK : **cumulative ACK** -> ex) ACK #100 : 99번까지 잘 받았다, 100번 줘!!!
    - **하나의 segment에 대한 ACK를 바로바로 보내준다**

- RTT(Round Trip Time)와 timeout
  - RTT : semgent 하나가 receiver에게 전달되고 ack가 sender에게 올 때까지 걸리는 시간
  - timeout 설정
    - timeout 발생하면 해당 segment를 **재전송**
    - timeout = sample RTT + margin
    - sample RTT를 구할 때 재전송한 것은 포함하지 않음
-  reliable transfer
  - 두 소켓 간의 TCP가 성립되면
    - 양쪽에 SEND BUFFER. RCV BUFFER생성(**bi-directional, full duplex**)
    - SEND BUFFER : **재전송**을 위해 임시 저장
      - SEND BASE : SEND BUFFER의 맨 앞 부분
        - **sliding window**
          - window size(**한 번에 보낼 수 있는 데이터 양**)  만큼 segment들을 한꺼번에 전송
          - ACK를 잘 받았으면 SEND BASE가 다음으로 이동(sliding)
          - 반대) stop and wait : segment를 하나 보내고 ack가 올바로 올때까지 기다림 
      - timer : SEND BASE에 존재 -> 재전송 용도
    - RCV BUFFER : **in-order**하게 segment를 전달
      - 중간에 segment가 유실된 경우 올때까지 기다렸다가 순서 맞으면 자신의 소켓(어플리케이션)에 전송
      - 호출은 에플리케이션에 의해 발생
  - pipelined segments
    - SENDER BUFFER에서 window size(**한 번에 보낼 수 있는 데이터 양**) 만큼 segment들 전송
  - cumulative ACK
    - ack #N : N-1번까지 잘 받은 상태, N번을 받아야되는 상태
    - **segment 각각에 대하여 ack를 보냄**
    - 중간에 segment가 유실된 경우 #seq가 더 큰 segment가 와도 유실된 segment의 #seq를 ack로 보냄
- fast retransmission 
  - 중복된 ack가 3번 (**실제로 4번**)오면 재전송함
  - timeout발생해서 재전송하는 것보다 빠름

<br>

#### TCP : Flow Control

- receiver의 상황에 맞게 sender측에서 segment 전송
- **rwnd : RCV BUFFER에서 남아있는 공간의 크기**
  - TCP header의 receive window에 rwnd의 크기가 담겨서 sender측에 전송된다
  - corner case : rwnd = 0;
    - sender측에서 segment를 보내지 않아 deadlock 발생할 수 있음
    - 해결 : probe 패킷(41바이트(헤더 40바이트+데이터 1바이트)) 전송
- Silly window syndrome -> segment사이즈를 어떻게 결정하는가
  - **segment사이즈가 작아지는 경우**

    - sender측의 앱에서 데이터를 천천히 만들거나
    - receiver측의 앱에서 받은 데이터 처리가 늦거나
  - segment사이즈는 클 수록 좋음 -> header크기에 대한 overhead감소
  - MSS (Maximum Segment Size) : 1500byte
  - 참고

    - Nagle 알고리즘(sender)
      - 목적 : 최대한 큰 segment를 보내자
      - 동작 방식
        - 처음엔 sender에서 일단 데이터를 보냄
        - sender에서 데이터를 모음 언제까지?
          - ack를 받거나 : 네트워크 상황이 좋음
          - MSS만큼 차거나 : buffer에 데이터를 담는게 더 빠름
      - 장점 : 네트워크 효울성 증가(생산하는 segment 수가 적음)
      - 단점 : ACK를 받을떄까지 대기하므로 전송 속도 느림

    - Clark's solution(receiver) : rwnd가 MSS보다 작으면 0 취급
    - Delayed ACK : ACK를 바로 보내지 않고, rwnd가 넉넉해질 떄까지 일정시간 기다렸다가 보냄

<br>

#### TCP : Connection Management

- **3-way-handshake(시작)**
- SYN (c->s) : 계세요? (**header만 보냄**)
  - SYN+ACK(s->c) : 예 있어요 (**header만 보냄**)
  - ACK(c->s) : 데이터 보내유 (established)
  
- **4-way handshake (종료)**

  - FIN (c->s) : 끝낼꼐유
  - ACK(s->c) : ㅇㅋㅇㅋ, client는 데이터 보내기 종료
  - FIN (s->c) : 끝났슈
  - ACK(c->s) : ㅇㅋㅇㅋ (client는 time wait 상태 - 자료 구조 해제X)

  - **마지막에 time wait가 필요한 이유**
    - cilent가 server로 보낸 ACK가 유실되면, server쪽에서 timeout이 발생하여 FIN을 다시 보냄

<br>

#### TCP : congestion control

> network 혼잡 상황에 맞게 보내는 패킷의 양을 조절

- congestion control이 없는 경우
  - 네트워크 패킷 증가로 인한 혼잡 발생
  - 재전송 발생 -> 더 많은 패킷 발생
  - 라우터 혼잡도 더욱 증가
  - 뻥......
  
- cwnd 
  - 현재 혼잡도를 가진 네트워크에 보낼 수 있는 패킷 수
  - 초기화 : 1MSS
  - sender의 window size = MIN(cwnd, rwnd)
  
- congestion control의 기본 철학
  
  - **많은 사람들이 네트워크를 골고루 사용할 수 있게 하자**
  - additive increase : 패킷 증가는 선형적으로 천천히
  - multiplicative decrease : loss 발생하면 보내는 패킷 수 **절반**으로 확 줄임
  
- slow start
  - 맨 처음에 패킷을 보낼 땐 1 MSS
  - 이후 2배씩 증가(2^n)
  - ssthresh(slow start threshold)에 도달하면 additive increase
    - ssthresh = cwnd 크기 / 2
      - 맨처음 TCP 통신 시작시 ssthresh = 0.5 MSS
  
- 패킷의 loss
  - 경우의 수
    - timer 발생 or 3 dup ACK
    - timer가 발생한 상황이 network 상황이 더 좋지 않다

- congestion control의 방법

  - TCP tahoe

    - sshthresh까지는 slow start
    - loss 발생시
      - cwnd가 1로 초기화
      - sshthresh = (loss 발생했을떄의 cwnd 크기) / 2

  - TCP reno
    - reno에선 3 dup ACK 발생한 경우는 multiplicative decrease 적용 -> cwnd를 반으로 줄임
    - time out 발생시 tahoe와 동일하게 행동

    