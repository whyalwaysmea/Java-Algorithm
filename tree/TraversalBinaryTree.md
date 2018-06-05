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
     * 递归 后续 
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