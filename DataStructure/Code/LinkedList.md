### LinkedList 구현

**코드**

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

