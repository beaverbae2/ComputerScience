### 최대힙 구현

**코드**

```java
import java.util.Arrays;

public class MyMaxHeapTest {
   public static void main(String[] args) {
      MyBinaryMaxHeap max_heap = new MyBinaryMaxHeap(10);
      int[] data2 = {10, 9, 7, 5, 6, 4, 2, 11, 8};
      for (int d : data2) {
         max_heap.add(d);
      }
      max_heap.printHeap();
      
      System.out.println(max_heap.poll());
      System.out.println(max_heap.poll());
      System.out.println(max_heap.poll());
      
      int[] arr1 = {16, 4, 10, 14, 7, 9, 3, 2, 8, 1, 11};
      heap_sort1(arr1);
      int[] arr2 = {16, 4, 10, 14, 7, 9, 3, 2, 8, 1, 11};
      heap_sort2(arr2);
   }
   
   public static void heap_sort1(int[] arr) {
      MyBinaryMaxHeap max_heap = new MyBinaryMaxHeap(arr.length);
      
      // 최대힙 구성
      for (int data : arr) {
         max_heap.add(data);
      }
      
      for (int i = 0; i < arr.length; i++) {
         max_heap.poll();
      }
      
      max_heap.printHeap();
   }
   
   public static void heap_sort2(int[] arr) {
      MyBinaryMaxHeap max_heap = new MyBinaryMaxHeap(arr);
      
      // 최대힙 구성
      int idx = arr.length - 1;
      int p_idx = (idx - 1) / 2;
      
      for (int i = p_idx; i >= 0; i--) {
         max_heap.heapify(i);
      }
      
      for (int i = idx; i >= 0; i--) {
         max_heap.swap(0, i);
         max_heap.decreaseSize();
         max_heap.heapify(0);
      }
      
      max_heap.printHeap();;
   }
}

class MyBinaryMaxHeap {
   private int[] arr;
   private int size;
   
   public MyBinaryMaxHeap(int max_size) {
      this.arr = new int[max_size];
   }
   
   public MyBinaryMaxHeap(int[] arr) {
      this.arr = arr;
      this.size = arr.length;
   }
   
   public void add(int data) {
      // 맨 뒤 인덱스에 값 추가
      arr[size] = data;
      
      int idx = size;
      int p_idx = (idx-1) / 2;
      
      while(p_idx >= 0 && arr[p_idx] < arr[idx]) {
         swap(p_idx, idx);
         idx = p_idx;
         p_idx = (idx - 1) / 2;
      }
      
      size++;
   }
   
   public int poll()  {
      int result = arr[size - 1];
      swap(0, size - 1);
      size--;
      heapify(0);
      return result;
   }
   
   public void heapify(int p_idx) {
      int max = arr[p_idx];
      int idx = p_idx;
      
      int left_idx = 2 * p_idx + 1;
      if (left_idx < size && max < arr[left_idx]) {
         idx = left_idx;
         max = arr[left_idx];
      }
      
      int right_idx = left_idx + 1;
      if (right_idx < size && max < arr[right_idx]) {
         idx = right_idx;
         max = arr[right_idx];
      }
      
      if (p_idx != idx) {
         swap(p_idx, idx);
         heapify(idx);
      }
   }
   
   
   public void swap(int a, int b) {
      int temp = arr[a];
      arr[a] = arr[b];
      arr[b] = temp;
   }
   
   public int size() {
      return size;
   }
   
   public void decreaseSize() {
      size--;
   }
   
   public void printHeap() {
      System.out.println(Arrays.toString(arr));
   }
}
```

