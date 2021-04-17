### 최소힙 구현

**코드**

```java
import java.util.Arrays;

/**
 * MinHeap
 * @author beaverbae
 * @see https://wonit.tistory.com/203
 */

public class Test {
	public static void main(String[] args) {
		BinaryMinHeap min_heap = new BinaryMinHeap(8);
		System.out.println(min_heap.poll());
		int[] data = {2, 4, 6, 5, 7, 9, 10, 1};
		for (int d : data) {
			min_heap.add(d);
		}
		System.out.println(Arrays.toString(min_heap.arr));
		System.out.println(min_heap.poll());
		System.out.println(Arrays.toString(min_heap.arr));
		System.out.println(min_heap.poll());
		System.out.println(Arrays.toString(min_heap.arr));
		System.out.println(min_heap.poll());
		System.out.println(Arrays.toString(min_heap.arr));
		System.out.println(min_heap.poll());
		System.out.println(Arrays.toString(min_heap.arr));
	}
}

class BinaryMinHeap {
	int[] arr;// 원소 저장
	int size;// 배열의 크기
	int cnt;// 배열에 저장된 원소의 개수
	
	{
		cnt = 0;
	}
	
	public BinaryMinHeap(int size) {
		this.arr = new int[size];
		this.size = size;
	}
	
	// 추가
	public void add(int data) {
		if (cnt == size) {
			System.out.println("heap is full!");
			return;
		}
		int idx = cnt;
		arr[idx] = data;// 데이터 삽입
		
		int p_idx = idx/2;// 부모 인덱스 
		while (arr[p_idx] > arr[idx]) {// 부모 노드의 값이 현재 노드의 값보다 크다면 swap
			swap(idx, p_idx);
			idx = p_idx;
			p_idx = p_idx/2;
		}
		cnt++;
	}
	
	// 삭제(맨 앞)
	public int poll() {
		if (cnt == 0) {
			System.out.println("heap is empty!");
			return -1;// 불가능
		}
		
		int v = arr[0];
		int p_idx = 0;// 부모 인덱스
		arr[0] = arr[cnt-1];
		
		while (true) {
			int left = 2*p_idx+1;// 왼쪽 자식 인덱스
			
			if (!isValidIndex(left)) break;
			
			int min = arr[left]; 
			int idx = left;
			int right = left+1;// 오른쪽 자식 인덱스
			
			// 왼쪽과 오른쪽 중 더 작은 노드를 선택
			if (isValidIndex(right) && arr[left] > arr[right]) {
				min = arr[right];
				idx = right;
			}
			
			// 부모 노드 값이 작다면 탐색 종료
			if (arr[p_idx] < min) {
				break;
			}
			
			// 아니면 부모노드와 자식 노드의 값을 swap
			swap(idx, p_idx);
			p_idx = idx;
		}
		
		arr[cnt-1] = 0;
		cnt--;
		return v;
	}
	
	public boolean isValidIndex(int idx) {
		return idx >=0 && idx < cnt;
	}
	
	public void swap(int a, int b) {
		int temp = arr[a];
		arr[a] = arr[b];
		arr[b] = temp;
	}
}

```

