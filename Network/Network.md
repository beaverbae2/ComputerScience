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
  - ACK(s->c) : ㅇㅋ보낸 데이터 다 받을 때까지 기다릴께유
  - FIN (s->c) : 끝났슈
  - ACK(c->s) : ㅇㅋㅇㅋ (client는 time wait 상태 -> 서버로 부터 받지 못한 data 기다림)