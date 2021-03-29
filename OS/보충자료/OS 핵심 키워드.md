**목적, 상황을 떠올리며 잘 기억하자**

<br>

#### 프로세스 관리

- CPU는 1개, 프로세스는 여러개 존재함을 항상 유념 -> 자원은 한정적이다..

- 프로세스란
- 프로세스의 구조
  - C 
  - 자바
- 프로세스 상태
  - 5가지 상태
  - time sharing system에서의 상태
  - [ready와 waiting의 차이점](https://jhnyang.tistory.com/7)

- PCB

  - 구조
  - context switching

- 스케줄러

  - 프로세스 큐 종류
    - Job Queue
    - Ready Queue
    - Device Queue

  - 스케줄러 종류

    - 프로세스 상태와 연결해서 생각하자
    - Job scheduler(Long-term scheduler)
    - CPU scheduler(Short-term scheduler)
      - [dispatcher와 scheduler의 차이](https://clucle.tistory.com/entry/Scheduler%EC%99%80-Dispatcher%EC%9D%98-%EC%B0%A8%EC%9D%B4)
    - Swapper(Mid-term scheduler)
      - suspended (blocked과 차이점)

  - CPU scheduler

    - 각각의 동작 방식, 장단점, burst time

    - FCFS
    - SJF
    - SRTF

    - Priority
    - Round Robin : time quantum, context switching

<br>

#### 쓰레드

- 구조
- 멀티 쓰레드 vs 멀티 프로세스

<br>

#### 프로세스 동기화

- 목적 3가지

- 임계 구역(Critical section) 문제
  - 원인
  - lock
  - [해결 방법 3가지](https://hongku.tistory.com/17)
- 세마포어
  - 구조
  - Mutual exclusion, ordering
- 전통적 동기화 문제
  - Producer-Consumer
  - Readers-Writers
  - Dining Philosopher -> deadlock

- dealock
  - 발생 조건 4가지(4가지 모두 충족해야 발생 가능)
  - 자원할당도
  - deadlock 방지 -> Dining philosopher문제 해결
  - deadlock 회피
  - deadlock 검출 및 복구
  - deadlock무시

- 모니터
  - 구조
  - mutual exclusion을 위한 키워드 
  - 조건동기를 위한 메소드 3가지

- 자바의 쓰레드 동기화
  - synchronized
  - volatile
  - atomic class

