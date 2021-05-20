### Binary Search Tree 구현

**코드**

```java

/**
 * Binary Search Tree
 * @author beaverbae
 * @see https://kim6394.tistory.com/223
 * @see https://yaboong.github.io/data-structures/2018/02/12/binary-search-tree/
 * 
 */
public class MyBinarySearchTreeTest {
   public static void main(String[] args) {
      MyBinarySearchTree tree = new MyBinarySearchTree();
      tree.insert(2);
      tree.insert(1);
      tree.insert(10);
      tree.insert(5);
      tree.insert(16);
      tree.insert(3);
      tree.insert(6);
      tree.insert(14);
      tree.insert(12);
      tree.insert(15);
      tree.insert(13);

      tree.callPreOrder();
      tree.callInOrder();
      tree.callPostOrder();
      
      tree.search(6);
      tree.search(1000);

      tree.delete(3);
      tree.callPreOrder();

//      tree.delete(12);
//      tree.callPreOrder();
//
//      tree.delete(16);
//      tree.callPreOrder();

      tree.delete(10);
      tree.callPreOrder();

   }
}

class MyBinarySearchTree {
   private Node root;
   private StringBuilder sb = new StringBuilder();
   
   class Node {
      private int data;
      private Node left, right;
      
      public Node(int data) {
         this.data = data;
      }
   }
   
   // 삽입
   public void insert(int data) {
      if (root == null) {
         root = new Node(data);
      } else {
         Node node = root;
         
         while(true) {
            if (node.data > data) {
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
   }
   
   public Node search(int data) {
      Node node = root;
      
      while (node != null) {
         if (node.data > data) {
            node = node.left;
         } else if (node.data < data) {
            node = node.right;
         } else {
            System.out.println("search : " + data + " is in tree");
            return node;
         }
      }
      
      System.out.println("search : " + data + " is not in tree");
      return null;
   }
   
   public Node delete(int data) {
      Node node = root;
      Node parentNode = root;
      boolean isLeftChild = false;
      
      while (node.data != data) {
         parentNode = node;
         
         if (node.data > data) { 
            node = node.left;
            isLeftChild = true;
         } else {
            node = node.right;
            isLeftChild = false;
         }
         
         if (node == null) {
            System.out.println("delete : " + data + " is not in tree");
            return null;
         }
      }
      
      if (node.left == null && node.right == null) { // 자식이 없는 경우
         if (node == root) {
            root = null;
         } else {
            if (isLeftChild) {
               parentNode.left = null;
            } else {
               parentNode.right = null;
            }
         }
      } else if (node.left == null) {// 오른쪽 자식만 있는 경우
         Node replaceNode = node.right;
         
         if (node == root) {
            root = replaceNode;
         } else {
            if (isLeftChild) {
               parentNode.left = replaceNode;
            } else {
               parentNode.right = replaceNode;
            }
         }
      } else if (node.right == null) {// 왼쪽 자식만 있는 경구
         Node replaceNode = node.left;
         
         if (node == root) {
            root = replaceNode;
         } else {
            if (isLeftChild) {
               parentNode.left = replaceNode;
            } else {
               parentNode.right = replaceNode;
            }
         }
      } else {// 자식이 둘 다 있는 경우
         Node replaceNode = getRightMinNode(node.right);
         
         if (node == root) {
            root = replaceNode;
         } else {
            if (isLeftChild) {
               parentNode.left = replaceNode;
            } else {
               parentNode.right = replaceNode;
            }
         }
         
         replaceNode.left = node.left;
      }
      
      System.out.println("delete : "+data);
      return node;
   }
   
   private Node getRightMinNode(Node right) {
      Node parentNode = right;
      Node node = right;
      
      while (node.left != null) {
         parentNode = node;
         node = node.left;
      }
      
      if (node != right) {
         parentNode.left = node.right;
         node.right = right;
      }
      
      return node;
   }
   
   public void callPreOrder() {
      preOrder(getRoot());
      System.out.println(sb.toString());
      sb = new StringBuilder();
   }
   
   public void preOrder(Node node) {
      if (node != null) {
         sb.append(node.data).append(" ");
         preOrder(node.left);
         preOrder(node.right);
      }
   }
   
   public void callInOrder() {
      inOrder(getRoot());
      System.out.println(sb.toString());
      sb = new StringBuilder();
   }
   
   public void inOrder(Node node) {
      if (node != null) {
         inOrder(node.left);
         sb.append(node.data).append(" ");
         inOrder(node.right);
      }
   }
   
   public void callPostOrder() {
      postOrder(getRoot());
      System.out.println(sb.toString());
      sb = new StringBuilder();
   }
   
   public void postOrder(Node node) {
      if (node != null) {
         postOrder(node.left);
         postOrder(node.right);
         sb.append(node.data).append(" ");
      }
   }
   
   private Node getRoot() {
      return root;
   }
}
```

