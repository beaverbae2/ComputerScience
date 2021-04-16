### Stack과 Queue

**코드 - 배열로 stack 구현, stack으로 queue 구현**

```java

/**
 * 1. 배열을 활용하여 stack 구현
 * 2. 1에서 구현한 stack 2개로 queue 구현
 * 
 * @author beaverbae
 *
 */

public class Test {
	public static void main(String[] args) {
		MyStack stack = new MyStack(3);
		stack.push(1);
		stack.push(2);
		System.out.println(stack.pop());
		stack.push(3);
		stack.push(4);
		stack.push(5);
		System.out.println(stack.pop());
		System.out.println(stack.pop());
		System.out.println(stack.pop());
		System.out.println(stack.pop());
		System.out.println(stack.pop());
		
		MyQueue queue = new MyQueue(3);
		queue.offer(1);
		queue.offer(2);
		queue.offer(3);
		System.out.println(queue.poll());
		queue.offer(4);
		System.out.println(queue.poll());
		System.out.println(queue.poll());
		System.out.println(queue.poll());
		System.out.println(queue.poll());
	}
}

// 배열로 stack구현
class MyStack {
	int[] arr;
	int size;
	int top;
	
	{
		top = 0;
	}
	
	public MyStack(int size) {
		this.arr = new int[size];
		this.size = size;
	}
	
	public void push(int data) throws FullException {
		if (isFull()) {
			throw new FullException("stack is full!");
		} else {
			arr[top++] = data;
		}
	}
	
	public int pop() throws EmptyException {
		if (isEmpty()) {
			throw new EmptyException("stack is empty!");
		} else {
			top--;
			return arr[top];
		}
	}
	
	public boolean isEmpty() {
		if (top == 0) return true;
		return false;
	}
	
	public boolean isFull() {
		if (top == size) return true;
		return false;
	}
}

class EmptyException extends RuntimeException{
	/**
	 * add this because Serializable
	 */
	private static final long serialVersionUID = 1L;

	public EmptyException() {
		super();
	}
	
	public EmptyException(String msg) {
		super(msg);
	}
}

class FullException extends RuntimeException{
	/**
	 * add this because Serializable
	 */
	private static final long serialVersionUID = 1L;

	public FullException() {
		super();
	}
	
	public FullException(String msg) {
		super(msg);
	}
}

// MyStack 2개로 Queue를 구현
class MyQueue {
	MyStack stack1, stack2;
	
	public MyQueue(int size) {
		this.stack1 = new MyStack(size);
		this.stack2 = new MyStack(size);
	}

	public void offer(int data) {
		stack1.push(data);
	}
	
	public int poll() {
		while (!stack1.isEmpty()) {
			stack2.push(stack1.pop());
		}
		
		int v = stack2.pop();
		
		while (!stack2.isEmpty()) {
			stack1.push(stack2.pop());
		}
		
		return v;
	}
}

```

