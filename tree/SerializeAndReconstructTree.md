# 二叉树的序列化和反序列化 
序列化和反序列化主要就是一个规则的制定  
我们这里以"!"来进行节点之间的分割，"#"表示空节点  

序列化和反序列化还需要一个统一的顺序，比如序列化的时候是按照中序遍历的方式来生成结果的，那么反序列化的时候也需要按照中序的顺序来生成树的结构   

为什么需要"!"进行节点分割，因为一咕噜的把节点值放字符串中，就没法还原了 

为什么需要"#"来表示空节点，因为如果有连续几个值相等的节点，这个时候只有根据完整的孩子结构来还原  



# 算法实现
```java
public class SerializeAndReconstructTree {

    public static class Node {
        public int value;
        public Node left;
        public Node right;

        public Node(int data) {
            this.value = data;
        }
    }

    public static String serializeByLevel(Node node) {
        String result = "";
        if(node == null) {
            return "#!";
        }
        Queue<Node> queue = new LinkedList<Node>();
        queue.offer(node);
        while(!queue.isEmpty()) {
            Node poll = queue.poll();
            
            if(poll == null) {
                result += "#!";
            } else {
                result += poll.value + "!";	
                queue.offer(poll.left);
                queue.offer(poll.right);
            }
            
        }
        return result;
    }

    public static Node reconstruct(String serializeResult) {
        String[] values = serializeResult.split("!");
        int index = 0;
        Node head = generateNodeByString(values[index++]);
        if(head == null) {
            return null;
        }
        Queue<Node> queue = new LinkedList<>();
        queue.offer(head);
        while(!queue.isEmpty()) {
            Node poll = queue.poll();
            Node left = generateNodeByString(values[index++]);
            if(left != null) {
                queue.offer(left);
            }
            Node right = generateNodeByString(values[index++]);
            if(right != null) {
                queue.offer(right);
            }
            poll.left = left;
            poll.right = right;
        }
        return head;
    }

    public static Node generateNodeByString(String val) {
        if("#".equals(val)) {
            return null;
        }
        return new Node(Integer.valueOf(val));
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

        
        String result = serializeByLevel(head);
        System.out.println(result);
        Node reconstruct = reconstruct(result);
        printTree(reconstruct);
    }

    // for test -- print tree
    public static void printTree(Node head) {
        System.out.println("Binary Tree:");
        printInOrder(head, 0, "H", 17);
        System.out.println();
    }

    public static void printInOrder(Node head, int height, String to, int len) {
        if (head == null) {
            return;
        }
        printInOrder(head.right, height + 1, "v", len);
        String val = to + head.value + to;
        int lenM = val.length();
        int lenL = (len - lenM) / 2;
        int lenR = len - lenM - lenL;
        val = getSpace(lenL) + val + getSpace(lenR);
        System.out.println(getSpace(height * len) + val);
        printInOrder(head.left, height + 1, "^", len);
    }

    public static String getSpace(int num) {
        String space = " ";
        StringBuffer buf = new StringBuffer("");
        for (int i = 0; i < num; i++) {
            buf.append(space);
        }
        return buf.toString();
    }
}
```