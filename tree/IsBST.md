# 判断一颗二叉树是否是二叉搜索树 
二叉查找树（Binary Search Tree），（又：二叉搜索树，二叉排序树）它或者是一棵空树，或者是具有下列性质的二叉树： 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值； 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值； 它的左、右子树也分别为二叉排序树。


# 思路 
因为左孩子小于节点，右孩子大于节点， 即 左 > 中 > 右，所以可以采用中序遍历的方式来处理 



# 实现 
```java
// 非递归
public class IsBST {

    public static class Node {
        public int value;
        public Node left;
        public Node right;

        public Node(int data) {
            this.value = data;
        }
    }

    public static boolean isBST(Node head) {
        if(head == null) {
            return true;
        }
        Stack<Node> stack = new Stack<Node>();
        stack.push(head);
        Integer lastValue = null; 
        while(!stack.isEmpty()) {
            if(head.left != null) {
                head = head.left;
                lastValue = head.value;
                stack.push(head);
            } else {
                Node pop = stack.pop();
                if(lastValue != null && lastValue > pop.value) {
                    return false;
                } else if(head.right != null){
                    head = head.right;
                    stack.push(head);
                }
                lastValue = pop.value;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        Node head = new Node(5);
        head.left = new Node(3);
        head.right = new Node(8);
        head.left.left = new Node(2);
        head.left.right = new Node(4);
        head.left.left.left = new Node(1);
        head.right.left = new Node(7);
        head.right.left.left = new Node(6);
        head.right.right = new Node(10);
        head.right.right.left = new Node(9);
        head.right.right.right = new Node(11);
        
        System.out.println(isBST(head));
    }
}
```
```java
// 递归中序
public class IsBST {

    public static class Node {
        public int value;
        public Node left;
        public Node right;

        public Node(int data) {
            this.value = data;
        }
    }

    private static Stack<Integer> stack;
	public static boolean isBST(Node head) {
		if(head == null) {
			return true;
		}
		stack = new Stack<Integer>();
		inOrder(head);
		
		int i = stack.pop();
        int j = Integer.MIN_VALUE;
        while (!stack.isEmpty()) {
            j = stack.pop();
            if (i <= j) {
                return false;
            }

            i = j;
        }

        return true;
	}
	
	public static void inOrder(Node root) {
        if (root != null) {
            inOrder(root.left);
            stack.push(root.value);
            inOrder(root.right);
        }
    }

    public static void main(String[] args) {
		Node head = new Node(5);
        head.left = new Node(3);
        head.right = new Node(8);
        head.left.left = new Node(2);
        head.left.right = new Node(4);
        head.left.left.left = new Node(1);
        head.right.left = new Node(7);
        head.right.left.left = new Node(6);
        head.right.right = new Node(10);
        head.right.right.left = new Node(9);
        head.right.right.right = new Node(11);
        
        System.out.println(isBST(head));
	}
}
```