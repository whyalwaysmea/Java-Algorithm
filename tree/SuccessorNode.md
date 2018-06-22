# 在二叉树中找到一个节点的后继节点  
【题目】现在有一种新的二叉树节点类型如下
```java
public class Node {
    public int value;
    public Node left;
    public Node right;
    public Node parent;
    public Node(int data) {
        this.value = data;
    }
}
``` 
该结构比普通二叉树节点结构多了一个指向父节点的parent指针。  
假设有一颗Node类型的节点组成的二叉树，树中每个节点的parent指针都正确地指向自己的父节点，头节点的parent指向null。只给一个在二叉树中的某个节点node，请实现返回node的后继节点的函数。 

在二叉树的中序遍历中，node的下一个节点就叫做node的后继节点 

# 思考 
## 遍历实现
既然是寻找后继节点，那么我们通过parent一层一层的往上先找到头节点，然后进行中序遍历，在遍历的过程中进行判断就好了 

## 直接寻找 
中序遍历的顺序是左根右，所以通过观察我们可以得出如下结论：  
主要是根据给定节点是否有右孩子，以及自身是父节点的左孩子还是右孩子判断   
 
1. 给定的节点有右孩子，那么右孩子的最左的子节点就是给定节点的后继   
2. 给定的节点是父节点的左孩子，那么父节点就是给定节点的后继  
3. 给定的节点是父节点的右孩子，那么向上寻找一个作为左孩子的祖先节点，这个祖先节点的父节点就是给定节点的后继    



# 算法实现
```java
public class SuccessorNode {

    public static class Node {
        public int value;
        public Node left;
        public Node right;
        public Node parent;

        public Node(int data) {
            this.value = data;
        }
    }

    public static Node getSuccessorNode(Node node) {
        if (node == null) {
            return node;
        }
        if (node.right != null) {
            return getLeftMost(node.right);
        } else {
            Node parent = node.parent;
            while (parent != null && parent.left != node) {
                node = parent;
                parent = node.parent;
            }
            return parent;
        }
    }

    public static Node getLeftMost(Node node) {
        if (node == null) {
            return node;
        }
        while (node.left != null) {
            node = node.left;
        }
        return node;
    }

    public static Node getSuccessorNode(Node node) {
        if (node == null) {
            return node;
        }
        Node head = node;
        while(head.parent != null) {
            head = head.parent;
        }
        
        if (head != null) {
            Stack<Node> stack = new Stack<Node>();
            boolean isSuccessor = false;
            while (!stack.isEmpty() || head != null) {
                if (head != null) {
                    stack.push(head);
                    head = head.left;
                } else {
                    head = stack.pop();
                    if(isSuccessor) {
                        return head;
                    }
                    if(head == node) {
                        isSuccessor = true;
                    }
                    
                    head = head.right;
                }
            }
        }
        
        return null;
    }

    public static void main(String[] args) {
        Node head = new Node(6);
        head.parent = null;
        head.left = new Node(3);
        head.left.parent = head;
        head.left.left = new Node(1);
        head.left.left.parent = head.left;
        head.left.left.right = new Node(2);
        head.left.left.right.parent = head.left.left;
        head.left.right = new Node(4);
        head.left.right.parent = head.left;
        head.left.right.right = new Node(5);
        head.left.right.right.parent = head.left.right;
        head.right = new Node(9);
        head.right.parent = head;
        head.right.left = new Node(8);
        head.right.left.parent = head.right;
        head.right.left.left = new Node(7);
        head.right.left.left.parent = head.right.left;
        head.right.right = new Node(10);
        head.right.right.parent = head.right;

        Node test = head.left.left;
        System.out.println(test.value + " next: " + getSuccessorNode(test).value);
        test = head.left.left.right;
        System.out.println(test.value + " next: " + getSuccessorNode(test).value);
        test = head.left;
        System.out.println(test.value + " next: " + getSuccessorNode(test).value);
        test = head.left.right;
        System.out.println(test.value + " next: " + getSuccessorNode(test).value);
        test = head.left.right.right;
        System.out.println(test.value + " next: " + getSuccessorNode(test).value);
        test = head;
        System.out.println(test.value + " next: " + getSuccessorNode(test).value);
        test = head.right.left.left;
        System.out.println(test.value + " next: " + getSuccessorNode(test).value);
        test = head.right.left;
        System.out.println(test.value + " next: " + getSuccessorNode(test).value);
        test = head.right;
        System.out.println(test.value + " next: " + getSuccessorNode(test).value);
        test = head.right.right; // 10's next is null
        System.out.println(test.value + " next: " + getSuccessorNode(test));
    }
}
```

