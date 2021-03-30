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

### GET과 POST

- 참고자료
  - [기록하기 - GET과 POST의 차이](https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/)
  - [JaeYeopHan - GET과 POST](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network#http%EC%9D%98-get%EA%B3%BC-post-%EB%B9%84%EA%B5%90)

- **요약 : GET은 데이터 조회, POST는 데이터 생성/변경**

- GET 

  - Idempotent(멱등) : 동일한 연산을 여러번 하면 결과 동일
  - 서버에 동일한 요청을 여러번 하면 응답 동일
  - 주로 자원을 **조회**를 할 때 사용

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
    - `HTTP Request Message`의 Header의 URL 에 담겨서 전송
    - 파라미터와 값이 URL에 그대로 노출(보안에 취약)
    - 응답 데이터가 브라우저의 캐시에 저장

<br>

- POST

  - Non-Idempotent : 동일한 연산에 대해 결과가 다를 수 있음

  - 서버에 동일한 요청에 대해 응답이 다름

  - 주로 자원을 **생성/변경** 하기 위해 사용

  - 특징

    - `HTTP Request Message`의 Body에 데이터를 담아 전송

    - 데이터가 눈에 보이지 않음(그래도 개발자 도구로 내용 확인 가능함... 필요한 경우 암호화 진행)

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
- header : checksunm ack, nak

##### rdt 2.1

- 상황 
  - rdt 2.0에서 문제점 발생
  - ack나 nak가 유실 된다면?
    - sender는 계속 재전송
    - receiver는 패킷의 순서를 파악 못함
- 해결
  - sequence number
    - 패킷의 번호
    - **순서 파악 가능**
    - receiver 측에서 중복된 번호의 패킷은 버리고 ack만 함
- header : checksum, ack, nak, #seq

#### rdt 2.2

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

- [handshake 참고링크](https://asfirstalways.tistory.com/356)

- Three-way handshake (시작)
  - SYN (c->s) : 계세요?
  - SYN+ACK(s->c) : 예 있어요
  - ACK(c->s) : 데이터 보내유 (established)

- Four-way handshake (종료)
  - FIN (c->s) : 끝낼꼐유
  - ACK(s->c) : ㅇㅋ, 남은 데이터 보낼께유
  - FIN (s->c) : 끝났슈
  - ACK(c->s) : ㅇㅋㅇㅋ (client는 time wait 상태 -> 서버로 부터 받지 못한 data 기다림)

<br>

#### TCP : reliable transfer

> 신뢰할 수 없는 채널을 가진 상황에서, TCP가 신뢰성 있는 전송을 함

-  sequence number와 ACK
  -  segment를 순서대로 보내기 위해 사용
  -  #seq : segment의 첫번째 바이트 값
  -  ACK : **cumulative ACK** -> ex) ACK #100 : 99번까지 잘 받았다, 100번 줘!!!
  -  **하나의 segment에 대한 ACK를 바로바로 보내준다**

- RTT(Round Trip Time)와 timeout
  - RTT : semgent 하나가 receiver에게 전달되고 ack가 sender에게 올 때까지 걸리는 시간
  - timeout 설정
    - timeout 발생하면 해당 segment를 **재전송**
    - timeout = sample RTT + margin
    - sample RTT를 구할 때 재전송한 것은 포함하지 않음
- reliable transfer
  - 두 소켓 간의 TCP가 성립되면
    - 양쪽에 SEND BUFFER. RCV BUFFER생성(**bi-directional, full duplex**)
    - SEND BUFFER : **재전송**을 위해 임시 저장
      - SEND BASE : SEND BUFFER의 맨 앞 부분
        - ACK를 잘 받았으면 SEND BASE가 다음으로 이동
      - timer : SEND BASE에 존재 -> 재전송 용도
    - RCV BUFFER : **in-order**하게 segment를 전달
      - 중간에 segment가 유실된 경우 올때까지 기다렸다가 순서 맞으면 자신의 소켓(어플리케이션)에 전송
      - 호출은 에플리케이션에 의해 발생
  - pipelined segments
    - SENDER BUFFER에서 window size 만큼 segment들 전송
  - cumulative ACK
    - ack #N : N-1번까지 잘 받은 상태, N번을 받아야되는 상태
    - segment 각각에 대하여 ack를 보냄
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

  - segment사이즈는 클 수록 좋음 -> header크기에 대한 overhead감소

  - MSS (Maximum Segment Size) : 1500byte

  - **segment사이즈가 작아지는 경우**

    - sender측의 앱에서 데이터를 천천히 만들거나
    - receiver측의 앱에서 받은 데이터 처리가 늦거나

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
    - Delayed ACK : ACK를 바로 보내지 않고, 일정시간 기다렸다가 보냄

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
