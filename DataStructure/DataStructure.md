# Data Structure(자료 구조)



## LinkedList

### LinkedList

- 데이터와 포인터로 이루어진 선형 자료 구조
- 구조
  - 데이터 : 값
  - 포인터 : 다음 위치(주소) 또는 이전 위치(주소)
- 탐색 시간 복잡도 : O(N)

**[LinkedList 구현 코드](./Code/LinkedList.md)**

<br>

## Stack과 Queue

- Stack : LIFO 구조
  - push : data를 맨 뒤에 삽입
  - pop : 맨 뒤의 데이터 리턴하고 삭제
- Queue : FIFO 구조
  - offer : data를 맨 뒤에 삽입
  - poll : 맨 앞의 데이터 리턴 후 삭제 

**[Stack과 Queue 구현 코드](./Code/Stack과Queue.md)**

<br>

## Tree

### Tree

- 계층 구조를 가지는 비선형 자료구조
- 구성 요소: 노드(정점), 간선
- 용어 
  - 부모(parent), 자식(child)
  - root : 최상위 노드
  - leaf : 말단 노드, 자식이 없는 노드
  - level : 트리의 깊이(root : 0)
- 특징
  - 사이클이 없는 그래프
  - 노드가 N개이면 간선은 N-1개
  - 자식 노드는 하나의 부모 노드만을 가진다
  - 임의의 두 노드 사이의 경로는 유일

### Binary Tree

- 자식이 2개인 노드
- 용어
  - Perfectly Binary Tree(완전 포화 트리)
  - Complete Binary Tree(완전 이진 트리)
  - Full Binary Tree(정 이진 트리)

- **순회**
  - PreOrder(전위순회) : parent -> left -> right
  - InOrder(중위순회) : left -> parent -> right
  - PostOrder(후위순회) : left -> right -> parent **(루트노드가 맨마지막)**

### Binary Search Tree

- Binary Tree의 일종
- 특징
  - 노드의 키는 유일
  - left 노드 값 < parent 노드 값
  - right 노드 값 > parent 노드 값
  - 서브트리도 이진탐색트리
- 탐색 연산 시간 복잡도
  - 일반 :  O(log N)
  - Worst : O(N) -> 연결리스트와 같음
    - rebalancing -> red black tree, AVL,....

- 중위 순회 시 키의 오름차순으로 순회한다

**[BST 구현 코드](./Code/BinarySearchTree.md)**

<br>

## Heap

- 참고자료
  - [Heee's Blog - [자료구조] 힙(heap)이란](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html)
- Complete Binary Tree(완전 이진 트리)의 일종, 우선순위 큐를 구현하기 위해 만들어진 자료구조
- 특징
  - 최솟값이나 최댓값을 빠르게 찾을 수 있음
  - 반정렬상태
  - 중복된 값 허용
- 시간 복잡도 
  - 삽입 : O(log n)
  - 삭제 : O(log n)
  - 이진 트리이기 때문
- 종류
  - 최소힙
    - 부모노드의 값 <= 자식 노드의 값
  - 최대힙
    - 부모노드의 값 >= 자식 노드의 값
- 구현 - 최소힙 기준
  - 일반적으로 배열사용
    - 부모 인덱스 : `idx`
    - 왼쪽 자식 인덱스 : `2 * idx + 1`
    - 오른쪽 자식 인덱스 : `2 * idx + 2` 
  - 삽입
    - 마지막 노드에 삽입
    - `부모 노드 값 > 자식 노드`의 값이면 반복하며 swap
  - 삭제
    - 맨 앞 노드 삭제
    - 초기화
      - 맨 뒤 노드를 맨 앞(루트)으로 위치시킴
      - 부모 노드를 루트 노드로 설정
    -  탐색
      - 자식 노드 중 더 작은 값을 가지는 노드 찾기
      - `부모노드 >  위에서 찾은 값` 이면 부모 노드와 자식 노드를 swap

[최소힙 구현코드](./Code/MinHeap)



## JAVA Collections

### ArrayList vs LinkedList

**구조**

- ArrayList : 인덱스를 가지는 연속적인 배열구조
- LinkedList : 포인터를 활용하여 비연속적인 구조를 연결

**삽입/삭제/탐색**

- ArrayList 
  - 삽입 : 삽입 시 사이즈를 늘려주는 연산. 삽입 위치 뒤의 요소들의 인덱스를 뒤로 한 칸씩 밀어야함 
  - 삭제 : 삭제 후 삭제 된 인덱스를 채워야함
  - 탐색 : 인덱스로 바로 탐색
- LinkedList
  - 삽입 : 노드 생성 후 포인터로 연결
  - 삭제 : 삭제할 노드의 포인터를 해제
  - 탐색 : 순차 접근 -> 처음부터 탐색

**시간 복잡도**

|                            | ArrayList | LinkedList   |
| -------------------------- | --------- | ------------ |
| Indexing                   | O(1)      | O(N)         |
| Insert/Delete at beginning | O(n)      | O(1)         |
| Insert/Delete at end       | O(1)      | O(n) or O(1) |
| Insert/Delete in middle    | O(n)      | O(n)         |

**정리**

- 삽입/삭제 빈번한 경우 : LinkedList 사용
- 인덱싱을 자주하는 경우 : ArrayList 사용

<br>

### Set

> 중복되지 않는 원소들의 모임

#### HashSet vs LinkedHashSet vs TreeSet

- HashSet : 순서가 보장되지 않음
- LinkedHashSet : 순서가 보장된다 (LinkedList)
- TreeSet : 원소를 특정한 순서대로 저장 (red-black-tree). 기본적으로 오름차순
- Set에서 다음 원소를 탐색할 때는 Iterator를 사용 (for each 써도 된다) 

- 시간 복잡도

  |               | Add      | Contains | Next     |
  | ------------- | -------- | -------- | -------- |
  | HashSet       | O(1)     | O(1)     | O(h/n)   |
  | LinkedHashSet | O(1)     | O(1)     | O(1)     |
  | TreeSet       | O(log N) | O(log N) | O(log N) |

- 참고) 자바 내부적으로 Set은 Map으로 구현되어 있음

<br>

### Map

> 고유한 key와 key에 매핑되는 value으로 이루어진 원소쌍들의 모임
>
> key를 통해 value를 획득

#### HashMap vs LinkedHashMap vs TreeMap

- HashMap : 순서가 보장되지 않음
- LinkedHashMap : 순서가 보장된다 (LinkedList)
- TreeMap : 원소를 특정한 순서대로 저장 (red-black-tree). 기본적으로 오름차순
- Map에서 다음 원소를 탐색할 때는 Iterator를 사용 (for each 써도 된다) 

- 시간 복잡도

  |               | Get      | ContainsKey | Next     |
  | ------------- | -------- | ----------- | -------- |
  | HashMap       | O(1)     | O(1)        | O(h/n)   |
  | LinkedHashMap | O(1)     | O(1)        | O(1)     |
  | TreeMap       | O(log N) | O(log N)    | O(log N) |

<br>

## HashTable

### HashTable

**참고자료**

- https://mangkyu.tistory.com/102
- [https://velog.io/@cyranocoding/Hash-Hashing-Hash-Table%ED%95%B4%EC%8B%9C-%ED%95%B4%EC%8B%B1-%ED%95%B4%EC%8B%9C%ED%85%8C%EC%9D%B4%EB%B8%94-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%9D%98-%EC%9D%B4%ED%95%B4-6ijyonph6o](https://velog.io/@cyranocoding/Hash-Hashing-Hash-Table해시-해싱-해시테이블-자료구조의-이해-6ijyonph6o)

<br>

**주요 키워드 : key, value, bucket, hash, hash function**

<br>

**정의**

- key 1개와 value 1개가 1:1로 연결
- key를 통해 value 획득
- key는 고유하다

<br>

**구조**

- 구성요소
  - key : 고유값
  - hash function : key -> hash
  - bucket : hash를 인덱스로 하여 value를 저장하는 배열
- 동작
  - key가 hash function을 통해 hash로 변경
  - hash는 value와 1:1 매칭이 되어 bucket에 저장

<br>

**시간 복잡도**

- 삽입, 삭제, 검색
  - 평균 : O(1)
  - 최악 : O(N) -> 충돌발생시 가능

<br>

**해시 충돌 (Hash collision)**

- 서로 다른 두 개 이상의 key가 같은 hash를 같는 현상
- 무조건 발생한다
  - key는 무한개이고 hash는 유한하므로
- 해결방법
  - 서로 상보적임
  - 분리연결법(Separate Chaining)
    - 기존 값과 총돌 값을 list 또는 tree로 연결
    - 장점
      - 한정된 bucket을 효울적 사용가능
      - hash function의 성능에 덜 의존적
    - 단점
      - 한 hash에 자료가 쏠리면 검색 효율 저하
      - 외부 저장 공간 사용(list)
    - 삽입, 삭제, 탐색
      - 최적 : O(1)
      - 최악 : O(N) -> 하나의 해시에 모든 원소가 들어있는 경우
  - 개방주소법(Open Addressing)
    - 비어있는 해시를 찾아서 저장
    - 방법
      - 선형 탐색(Linear Probing) : 다음 hash(+1) or n개(+n)를 건너뛰어서 비어있는 hash에 저장
      - 제곱 탐색(Quadratic Probing) : 충돌 발생한 hash의 제곱을 한 hash에 저장
      - 이중 해시(Double Hashing) : 다시 해시 함수를 한 번 더 적용하여 새로운 hash 해당
    - 장점
      - 외부 저장 공간 필요 없음
    - 단점
      - hash function의 성능에 영향을 많이 받음(비어있는 hash를 얼마나 빨리 찾느냐...)
    - 삽입, 삭제, 탐색
      - 최적 : O(1)
      - 최악 : O(N)

<br>

**자바에서 Hash Table과 HashMap의 차이**

- Hash Table은 synchronized 키워드 붙어있음
- Hash Table은 병렬 프로그래밍시 동기화 지원
- HashMap은 자원X

<br>