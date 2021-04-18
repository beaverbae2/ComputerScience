### 최대힙 구현

**코드**

```java
import java.util.Arrays;

/**
 * MaxHeap
 * @author beaverbae
 * @see https://wonit.tistory.com/203
 * 
 */

public class Test {
	public static void main(String[] args) {
		BinaryMaxHeap max_heap = new BinaryMaxHeap(10);
		int[] data2 = {10, 9, 7, 5, 6, 4, 2, 11, 8};
		for (int d : data2) {
			max_heap.add(d);
		}
		System.out.println(Arrays.toString(max_heap.arr));
		System.out.println(max_heap.poll());
		System.out.println(Arrays.toString(max_heap.arr));
		System.out.println(max_heap.poll());
		System.out.println(Arrays.toString(max_heap.arr));
		System.out.println(max_heap.poll());
		System.out.println(Arrays.toString(max_heap.arr));
		
		int[] data3 = {16, 4, 10, 14, 7, 9, 3, 2, 8, 1, 11};
		System.out.println(Arrays.toString(heapsort1(data3, data3.length)));
		System.out.println(Arrays.toString(heapsort2(data3, data3.length)));
	}
	
	// heapsort1 : add, poll 활용
	public static int[] heapsort1(int[] arr, int size) {
		int[] result = new int[size];
		BinaryMaxHeap max_heap = new BinaryMaxHeap(size);
		
		// 최대 힙 구성 - add
		for (int data : arr) {
			max_heap.add(data);
		}
		
		// 오름차순 정렬 - poll
		for (int i = size-1; i >= 0; i--) {
			result[i] = max_heap.poll();
		}
		
		return result;
	}
	
	// heapsort2 : heapify 활용
	public static int[] heapsort2(int[] arr, int size) {
		BinaryMaxHeap max_heap = new BinaryMaxHeap(size);
		max_heap.arr= arr;
		max_heap.cnt = size;
	
		// 최대 힙 구성
		int idx = max_heap.cnt-1;
		int p_idx = (idx-1)/2;
		for (int i = p_idx; i >= 0; i--) {
			max_heap.heapify(i);
		}
		
		// 오름차순 정렬 - poll과 동일
		for (int i = idx; i >= 0; i--) {
			max_heap.swap(0, i);
			max_heap.cnt--;
			max_heap.heapify(0);
		}
		
		return max_heap.arr;
	}
}


class BinaryMaxHeap {
	int[] arr;
	int size;
	int cnt;
	
	{
		this.cnt = 0;
	}
	
	public BinaryMaxHeap(int size) {
		this.arr = new int[size];
		this.size = size;
	}
	
	public void add(int data) {
		if (cnt == size) {
			System.out.println("heap is full!");
			return;
		}
		
		arr[cnt] = data; // 맨 뒤 인덱스에 data 추가
		
		// heap 구조 유지
		int idx = cnt;
		int p_idx = (idx-1)/2;
		
		while (arr[p_idx] < arr[idx]) {
			swap(p_idx, idx);
			idx = p_idx;
			p_idx = (p_idx-1)/2;
		}
		
		cnt++;
	}
	
	public int poll() {
		if (cnt == 0) {
			System.out.println("heap is empty!");
			return -1;
		}
		int v = arr[0];
		swap(0, cnt-1);
		cnt--;
		heapify(0);
		arr[cnt] = 0;
		return v;
	}
	
	public void heapify(int p_idx) {
		int max = arr[p_idx];
		int idx = p_idx;
		
		// 왼쪽 노드 확인
		int left_idx = 2 * p_idx + 1;
		if (left_idx < cnt && max < arr[left_idx]) {
			max = arr[left_idx];
			idx = left_idx;
		}
		
		// 오른쪽 노드 확인
		int right_idx = left_idx + 1;
		if (right_idx < cnt && max < arr[right_idx]) {
			max = arr[right_idx];
			idx = right_idx;
		}
		
		if (idx != p_idx) {
			swap(p_idx, idx);
			heapify(idx);
		}
	}
	

	public void swap(int a, int b) {
		int temp = arr[a];
		arr[a] = arr[b];
		arr[b] = temp;
	}
}

```

