# 判断一棵二叉树是否是平衡二叉树  
首先说一下什么是平衡二叉树（AVL树）：它是一 棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树 

# 思路 
首先想到的就是递归，通过判断每一颗子树是否是平衡二叉树，如果是，在返回该子树的高度，然后让它的父节点，去比较左右子树的高度差的绝对值值；如果不是，则可以直接结束判断

所以在递归的过程中需要返回两个值，一个是是否是平衡二叉树，一个是该子树的节点高度  


# 实现  
```java
public class IsBalancedTree {

    public static class Node {
        public int value;
        public Node left;
        public Node right;

        public Node(int data) {
            this.value = data;
        }
    }

    public static class ReturnData {
        public boolean isB;
        public int h;
        public ReturnData(boolean isB, int h) {
            this.isB = isB;
            this.h = h;
        }
    }

    public static ReturnData process(Node head) {
        if(head == null) {
            return new ReturnData(true, 0);
        }
        ReturnData leftData = process(head.left);
        if(!leftData.isB) {
            return new ReturnData(false, 0);
        }
        
        ReturnData rightData = process(head.right);
        if(!rightData.isB) {
            return new ReturnData(false, 0);
        }
        
        if(Math.abs(leftData.h - rightData.h) > 1) {
            return new ReturnData(false, 0);
        }
        return new ReturnData(true, Math.max(leftData.h, rightData.h) + 1);
    }

    public static boolean isBalance(Node head) {
        return process(head).isB;
    }

    /**
     *           1
     *     2          3
     *  4     5    6       7
     */
    public static void main(String[] args) {
        Node head = new Node(1);
        head.left = new Node(2);
        head.right = new Node(3);
        head.left.left = new Node(4);
        head.left.right = new Node(5);
        head.right.left = new Node(6);
        head.right.right = new Node(7);

        System.out.println(isBalance(head));

    }
}
```