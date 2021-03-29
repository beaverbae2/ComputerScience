# Data Structure(자료 구조)





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
  - right 노그 값 > parent 노드 값
  - 서브트리도 이진탐색트리
- 탐색 연산 시간 복잡도
  - 일반 :  O(log N)
  - Worst : O(N) -> 연결리스트와 같음
    - rebalancing -> red black tree, AVL,....

- 중위 순회 시 키의 오름차순으로 순회한다