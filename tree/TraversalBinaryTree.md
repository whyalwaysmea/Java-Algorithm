# 实现二叉树的先序，中序，后序遍历  
二叉树的遍历分为以下三种：

先序遍历：遍历顺序规则为【根左右】  

中序遍历：遍历顺序规则为【左根右】  

后序遍历：遍历顺序规则为【左右根】  

什么是【根左右】？就是先遍历根，再遍历左孩子，最后遍历右孩子；

![](https://img-blog.csdn.net/20161110202932907?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) 

先序遍历：ABCDEFGHK

中序遍历：BDCAEHGKF

后序遍历：DCBHKGFEA

# 递归实现  
```java
public class PreInPosTraversal {

    public static class Node {
        public int value;
        public Node left;
        public Node right;

        public Node(int data) {
            this.value = data;
        }
    }

    /**
     * 递归 前序 
     */
    public static void preOrderRecur(Node head) {
        if(head == null) {
            return ;
        }
        // 输出根节点 
        System.out.print(head.value + " ");
        // 遍历左孩子 
        preOrderRecur(head.left);
        // 遍历右孩子
        preOrderRecur(head.right);
    }

    /**
     * 递归 中序 
     */
    public static void inOrderRecur(Node head) {
        if(head == null) {
            return ;
        }		
        // 遍历左孩子 
        inOrderRecur(head.left);
        System.out.print(head.value + " ");
        // 遍历右孩子
        inOrderRecur(head.right);
    }

    /**
     * 递归 后序
     */
    public static void posOrderRecur(Node head) {
        if(head == null) {
            return ;
        }		
        // 遍历左孩子 
        posOrderRecur(head.left);
        // 遍历右孩子
        posOrderRecur(head.right);
        System.out.print(head.value + " ");
    }    

    /**
     *                5
     *           3         8 
     *        2    4     7   10
     *     1            6    9 11
     *     
     *     前序： 5 3 2 1 4 8 7 6 10 9 11
     *     中序： 1 2 3 4 5 6 7 8 9 10 11
     *     后序: 1 2 4 3 6 7 9 11 10 8 5
     */
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

        // recursive
        System.out.println("==============recursive==============");
        System.out.print("pre-order: ");
        preOrderRecur(head);
        System.out.println();
        System.out.print("in-order: ");
        inOrderRecur(head);
        System.out.println();
        System.out.print("pos-order: ");
        posOrderRecur(head);
        System.out.println();

    }

}
```

# 非递归实现  

## 思路  
我们首先要知道任何递归形式的方法都可以改成非递归的，递归的好处在于它保存了方法调用的栈结构，当然这也会导致递归性能上的问题  
所以我们也可以用一个栈来记录方法的调用  

**前序遍历：**  
前序遍历的顺序是根左右，再结合上栈的结构特点，所以我们可以先把右孩子放进栈，再把左孩子放进栈。通过循环出栈，出栈后立即打印，然后再将该节点的右孩子、左孩子放进栈。  

**中序遍历：** 
中序遍历的顺序是左根右，所以我们就先一直放左孩子，直到没有左孩子了，此时就出栈，然后打印，再将它右孩子的左孩子一直进栈。 



# 非递归的实现
```java
public class PreInPosTraversal {

    public static class Node {
        public int value;
        public Node left;
        public Node right;

        public Node(int data) {
            this.value = data;
        }
    }		

    /**
     * 前序 非递归
     */
    public static void preOrderUnRecur(Node head) {
        if (head != null) {
            Stack<Node> stack = new Stack<Node>();
            stack.add(head);
            while (!stack.isEmpty()) {
                head = stack.pop();
                System.out.print(head.value + " ");
                if (head.right != null) {
                    stack.push(head.right);
                }
                if (head.left != null) {
                    stack.push(head.left);
                }
            }
        }
    }

    /**
     * 中序  非递归
     */
    public static void inOrderUnRecur(Node head) {
        if (head != null) {
            Stack<Node> stack = new Stack<Node>();
            while (!stack.isEmpty() || head != null) {
                if (head != null) {
                    stack.push(head);
                    head = head.left;
                } else {
                    head = stack.pop();
                    System.out.print(head.value + " ");
                    head = head.right;
                }
            }
        }
    }

    /**
     * 后序 非递归
     */
    public static void posOrderUnRecur1(Node head) {
        if (head != null) {
            Stack<Node> s1 = new Stack<Node>();
            Stack<Node> s2 = new Stack<Node>();
            s1.push(head);
            while (!s1.isEmpty()) {
                head = s1.pop();
                s2.push(head);
                if (head.left != null) {
                    s1.push(head.left);
                }
                if (head.right != null) {
                    s1.push(head.right);
                }
            }
            while (!s2.isEmpty()) {
                System.out.print(s2.pop().value + " ");
            }
        }
    }

    /**
     * 后序 非递归
     */
    public static void posOrderUnRecur2(Node head) {
        if (head != null) {
            Stack<Node> stack = new Stack<Node>();
            stack.push(head);
            Node c = null;
            while (!stack.isEmpty()) {
                c = stack.peek();
                if(c.left != null && c.left != head && c.right != head) {
                    stack.push(c.left);
                } else if(c.right != null && head != c.right) {
                    stack.push(c.right);
                } else {
                    System.out.print(stack.pop().value + " ");
                    head = c;
                }
            }
        }
    }

    /**
     *                5
     *           3         8 
     *        2    4     7   10
     *     1            6    9 11
     *     
     *     前序： 5 3 2 1 4 8 7 6 10 9 11
     *     中序： 1 2 3 4 5 6 7 8 9 10 11
     *     后序: 1 2 4 3 6 7 9 11 10 8 5
     */
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


        // unrecursive
        System.out.println("============unrecursive=============");
        System.out.print("pre-order: ");
        preOrderUnRecur(head);
        System.out.print("in-order: ");
        inOrderUnRecur(head);
        System.out.print("pos-order: ");
        posOrderUnRecur1(head);
        System.out.println();
        posOrderUnRecur2(head);

    }

}
```