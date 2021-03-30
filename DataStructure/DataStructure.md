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

