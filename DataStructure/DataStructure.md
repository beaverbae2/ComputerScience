# Data Structure(자료 구조)



## LinkedList

### LinkedList

- 데이터와 포인터로 이루어진 선형 자료 구조
- 구조
  - 데이터 : 값
  - 포인터 : 다음 위치(주소) 또는 이전 위치(주소)
- 탐색 시간 복잡도 : O(N)

**구현 코드**

```java
// Doubly Linked List : 삽입, 삭제, 탐색
public class Test {
	public static void main(String[] args) {
		DoublyLinkedList list = new DoublyLinkedList();
		list.addFirst(2);
		list.addFirst(1);
		list.addFirst(0);
		list.addLast(3);
		list.addLast(4);
		list.addLast(5);
		list.add(1, 5);
		list.removeFirst();
		list.removeFirst();
		list.removeFirst();
		list.add(0, 1);
		list.removeLast();
		list.removeLast();
		list.removeLast();
		list.addLast(3);
		list.addLast(4);
		list.addLast(5);
		list.remove(1);
		list.remove(1);
		list.remove(1);
		System.out.println(list.print());
	}

}

class DoublyLinkedList {
	private Node head, tail;
	private int size;

	class Node {
		int data;
		Node prev, next;

		public Node(int data) {
			this.data = data;
		}
	}

	// 삽입
	// 첫번째 노드에 삽입
	public void addFirst(int data) {
		if (size == 0) {
			head = new Node(data);
			tail = head;
		} else {
			Node newNode = new Node(data);
			head.prev = newNode;
			newNode.next = head;
			head = newNode;
		}
		size++;
	}

	// 마지막 노드에 삽입
	public void addLast(int data) {
		if (size == 0) {
			addFirst(data);
		} else {
			Node newNode = new Node(data);
			tail.next = newNode;
			newNode.prev = tail;
			tail = newNode;
		}
		size++;
	}

	// 임의의 위치에 삽입
	public void add(int index, int data) {
		if (index >= size) {
			index = size - 1;
		}

		if (index == 0 || size == 0) {
			addFirst(data);
		} else if (index == size - 1) {
			addLast(data);
		} else {
			Node prevNode = get(index - 1);
			Node nextNode = prevNode.next;

			Node newNode = new Node(data);
			prevNode.next = newNode;
			newNode.prev = prevNode;

			nextNode.prev = newNode;
			newNode.next = nextNode;
			size++;
		}
	}

	// 삭제
	// 맨앞 삭제
	public Node removeFirst() {
		if (size == 0) return null;
		
		Node node = head;
		head = node.next;
		head.prev = null;
		size--;
		return node;
	}

	// 맨뒤 삭제
	public Node removeLast() {
		if (size == 0) return null;
		
		Node node = tail;
		tail = node.prev;
		tail.next = null;
		size--;
		return node;
	}

	// 임의의 위치 노드 삭제
	public Node remove(int index) {
		if (index >= size) {
			index = size - 1;
		}

		if (index <= 0) {
			return removeFirst();
		} else if (index == size - 1) {
			return removeLast();
		} else {
			Node node = get(index);
			Node prevNode = node.prev;
			Node nextNode = node.next;

			prevNode.next = nextNode;
			nextNode.prev = prevNode;
			size--;
			return node;
		}
	}
	
	public boolean isEmpty() {
		if (size == 0) return true;
		return false;
	}
	
	// size
	public int size() {
		return size;
	}

	// 출력
	public String print() {
		StringBuilder sb = new StringBuilder();
		Node node = head;
		while (true) {
			if (node == tail) {
				sb.append(node.data);
				break;
			} else {
				sb.append(node.data).append(", ");
				node = node.next;
			}
		}

		return sb.toString();
	}

	// 특정 위치 노드 리턴
	private Node get(int index) {
		int idx = 0;
		Node node = head;
		while (idx < index) {
			node = node.next;
			idx++;
		}

		return node;
	}
}

```

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

**구현코드**

```java
import BinarySearchTree.BinarySearchTree.Node;

/**
 * Binary Search Tree
 * @author beaverbae
 * @see https://kim6394.tistory.com/223
 * @see https://yaboong.github.io/data-structures/2018/02/12/binary-search-tree/
 * 
 */

public class Test {
	static StringBuilder sb;
	
	public static void main(String[] args) {
		BinarySearchTree tree = new BinarySearchTree();
		tree.insert(5);
		tree.insert(2);
		tree.insert(1);
		tree.insert(3);
		tree.insert(4);
		tree.insert(7);
		tree.insert(6);
//		tree.remove(6); // 자식이 없는 노드 삭제
//		tree.remove(7); // 자식이 하나인 노드 삭제
		tree.remove(2); // 자식이 둘인 노드 삭제
		
		sb = new StringBuilder();
		preOrder(tree.root);
		System.out.println(sb.toString());
		
		sb = new StringBuilder();
		inOrder(tree.root);
		System.out.println(sb.toString());
		
		sb = new StringBuilder();
		postOrder(tree.root);
		System.out.println(sb.toString());
	}
	
	public static void preOrder(Node node) {
		if (node != null) {
			sb.append(node.data);
			preOrder(node.left);
			preOrder(node.right);
		}
	}
	
	public static void inOrder(Node node) {
		if (node != null) {
			inOrder(node.left);
			sb.append(node.data);
			inOrder(node.right);
		}
	}
	
	public static void postOrder(Node node) {
		if (node != null) {
			postOrder(node.left);
			postOrder(node.right);
			sb.append(node.data);
		}
	}
}


class BinarySearchTree {
	Node root;
	
	class Node {
		int data;
		Node left, right;
		
		public Node(int data) {
			this.data = data;
		}
	}
	
	public void insert(int data) {
		if (root == null) {
			root = new Node(data);
			return;
		}
		
		Node node = root;
		while (true) {
			if (data < node.data) {
				if (node.left == null) {
					node.left = new Node(data);
					break;
				} else {
					node = node.left;
				}
			} else {
				if (node.right == null) {
					node.right = new Node(data);
					break;
				} else {
					node = node.right;
				}
			}
		}
	}
	
	public void remove(int data) {
		Node node = root;// 삭제할 노드
		Node parent = root;// 삭제할 노드의 부모
		boolean isLeftChild = false;// 삭제할 노드가 부모의 왼쪽인지
		
		// 삭제할 노드 찾기
		while (node.data != data) {
			parent = node;
			
			if (data < node.data) {
				node = node.left;
				isLeftChild = true;
			} else {
				node = node.right;
				isLeftChild = false;
			}
			
			if (node == null) return; // 값이 없는 경우
		}
		
		// 삭제할 노드가 자식이 아예 없는 경우
		if (node.left == null && node.right == null) {
			// 그대로 삭제하면 됨
			if (node == root) {
				root = null;
			} else if (isLeftChild) {
				parent.left = null;
			} else {
				parent.right = null;
			}
		}
		// 삭제할 노드가 자식이 하나 있는 경우
		// 왼쪽 자식만 있는 경우
		else if (node.right == null) {
			// 삭제할 노드의 왼쪽 서브트리를 삭제할 노드 자리에 둠
			if (node == root) {
				root = node.left;
			} else if (isLeftChild) {
				parent.left = node.left;
			} else {
				parent.right = node.left;
			}
		}
		
		// 오른쪽 자식만 있는 경우
		else if (node.left == null) {
			// 삭제할 노드의 왼쪽 서브트리를 삭제할 노드 자리에 둠
			if (node == root) {
				root = node.right;
			} else if (isLeftChild) {
				parent.left = node.right;
			} else {
				parent.right = node.right;
			}
		}
		// 삭제할 노드가 자식이 둘 다 있는 경우
		else {
			Node temp = getRightMinNode(node.right);// 오른쪽 서브트리에서 가장 작은 노드 찾기
			
			if (node == root) {
				root = temp;
			} else if (isLeftChild) {
				parent.left = temp;
			} else {
				parent.right = temp;
			}
			
			temp.left = node.left; //삭제할 노드의 왼쪾 서브트리와 연결
		}
	}

	private Node getRightMinNode(Node right) {
		Node parent = right;
		Node node = right;
		
		while (node.left != null) {
			parent = node;
			node = node.left;
		}
		
		parent.left = null;
		return node;
	}

}
```

<br>

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