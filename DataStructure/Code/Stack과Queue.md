### Stack과 Queue

**코드1 - 배열로 Stack 구현**

```java
public class MyStackTest {
	public static void main(String[] args) {
		MyStack myStack = new MyStack(10);
		
		for (int data = 1; data <= 5; data++) {
			myStack.push(data);
		}
		
		for (int data = 1; data <= 5; data++) {
			System.out.println(myStack.pop());
		}
		
		for (int data = 6; data <= 10; data++) {
			myStack.push(data);
		}
		
		for (int data = 6; data <= 10; data++) {
			System.out.println(myStack.pop());
		}
		
		System.out.println(myStack.pop());
		
		
		for (int data = 1; data <= 11; data++) {
			myStack.push(data);
		}
		
		for (int data = 1; data <= 11; data++) {
			System.out.println(myStack.pop());
		}
		
	}
}

// 배열로 구현
class MyStack {
	private int arr[];
	private int size;
	
	public MyStack(int n) {
		arr = new int[n];
	}
	
	public void push(int data) {
		if (size == arr.length) return;
		
		arr[size++] = data;
	}
	
	public int pop() {
		if (isEmpty()) return -1;
		
		size--;
		return arr[size];
	}
	
	public int size() {
		return size;
	}
	
	public boolean isEmpty() {
		return size == 0;
	}
}

```

<br>

**코드 2 - Stack으로 Queue 구현**

```java
public class MyQueueTest {
	public static void main(String[] args) {
		MyQueue myQueue = new MyQueue(10);
		
		for (int data = 1; data <= 5; data++) {
			myQueue.add(data);
		}
		
		for (int data = 1; data <= 5; data++) {
			System.out.println(myQueue.poll());
		}
		
		System.out.println();
		
		for (int data = 6; data <= 10; data++) {
			myQueue.add(data);
		}
		
		for (int data = 6; data <= 10; data++) {
			System.out.println(myQueue.poll());
		}
		
		System.out.println();
		System.out.println(myQueue.poll());
		System.out.println();
		
		for (int data = 1; data <= 11; data++) {
			myQueue.add(data);
		}
		
		for (int data = 1; data <= 11; data++) {
			System.out.println(myQueue.poll());
		}
	}
}

// 스택 2개로 큐 구현
class MyQueue {
	private MyStack stack1;
	private MyStack stack2;
	
	public MyQueue(int n) {
		stack1 = new MyStack(n);
		stack2 = new MyStack(n);
	}
	
	public void add(int data) {
		stack1.push(data);
	}
	
	public int poll() {
		int result = -1;
		
		while(!stack1.isEmpty()) {
			stack2.push(stack1.pop());
		}
		
		result = stack2.pop();
		
		while(!stack2.isEmpty()) {
			stack1.push(stack2.pop());
		}
		
		return result;
	}
	
}

```

