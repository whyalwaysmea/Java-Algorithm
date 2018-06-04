# 两个单链表相交的一系列问题  
【题目】在本题中，单链表可能有环，也可能无环。给定两个单链表的头节点head1和head2，这两个链表可能相交，也可能不相交。请实现一个函数，如果两个链表相交，则返回相交的第一个节点；如果不相交，则返回null。 

【要求】如果链表1的长度为N，链表2的长度为M，时间复杂度请达到O(N+M)，额外空间复杂度请达到O(1) 

# 思考 
其实这道题目是一系列的问题，拆分下来如下  
1. 判断单链表是否成环  
2. 对于成环的单链表，找出第一个入环的点   
3. 判断两个环是否相交  

所以我们先一步一步的实现。 

## 单链表是否成环 
首先我们可以通过借助外部数据结构的方式来完成。  
用Hash结构，遍历链表，如果hash表中没有值，那么将该节点放入hash表中。如果单链表成环，那么在遍历的过程中，肯定会遇到hash表中已存在节点的情况。  

如果不借助外部数据结构，那么我们可以通过快慢双指针的方式来判断  
一个快指针，一个慢指针，快指针一次走两步，慢指针一次走一步，如果链表成环，那么两个指针一定会相遇的 

## 入环的第一个节点  
第一种方式当然还是借助外部数据结构了  
用Hash结构，遍历链表并放入hash表中，放入之前先判断是否存在，如果出现了第一个存在的节点，那么它就是第一个入环的节点了 

第二种方式同样是使用快慢指针，但同时需要借助一个公式   
如果链表成环，那么快慢指针一定会相遇，那么在相遇的时候，快指针回到head节点，此时慢节点到入环点的距离等于head节点到入环点的距离

## 相交情况  
两个链表寻找相交点可以分为三种情况：  
1. 两个链表都不成环，那么就直接判断最后一个节点，如果最后一个节点相同，那么就肯定相交了  
2. 两个链表都成环，在入环之前相交  
3. 两个链表都成环，在分别成环之后相交，那么第一个相交点可以看作各自的入环点 

所以这里都需要分情况来判断的  
第一种情况很好判断是否相交，那么怎么获取第一个交点呢？ 我们可以通过计数的方式，这样可以得知哪一个链表更长以及连链表的长度差值N，然后再先将长链表移动N步，最后两个链表一起移动，那么相遇的那个地方就是第一个交点了  
对于第二种情况，如果在入环之前就相交了，那么它们的入环点就是相同的；第三种情况，入环点则不同      


# 实现 
## 判断单链表是否成环
```java
public class FindFirstIntersectNode {

    public static class Node {
        public int value;
        public Node next;

        public Node(int data) {
            this.value = data;
        }
    }

    /**
        * 判断单链表是否成环
        * 借助外部数据结构 
        */
    public static boolean isLoopNode1(Node head) {
        HashSet<Node> hash = new HashSet<Node>();
        while(head != null) {
            if(hash.contains(head)) {
                return true;
            }
            hash.add(head);
            head = head.next;
        }
        return false;
    }

    /**
        * 通过快慢指针的方式判断单链表是否成环
        */
    public static boolean isLoopNode2(Node head) {
        if(head == null || head.next == null || head.next.next == null) {
            return false;
        }
        Node fast = head.next.next;
        Node slow = head.next;
        while(fast != slow) {
            if(fast.next == null || fast.next.next == null) {
                return false;
            }
            fast = fast.next.next;
            slow = slow.next;
        }
        return true;
    }
		
}
```

## 获取第一个入环的节点 
```java
public class FindFirstIntersectNode {

    public static class Node {
        public int value;
        public Node next;

        public Node(int data) {
            this.value = data;
        }
    }

    public static Node getLoopNode(Node head) {
        HashSet<Node> hash = new HashSet<Node>();
        while(head != null) {
            if(hash.contains(head)) {
                return head;
            }
            hash.add(head);
            head = head.next;
        }
        return null;
    }

    public static Node getLoopNode2(Node head) {
        if (head == null || head.next == null || head.next.next == null) {
            return null;
        }
        Node n1 = head.next; // n1 -> slow
        Node n2 = head.next.next; // n2 -> fast
        while (n1 != n2) {
            if (n2.next == null || n2.next.next == null) {
                return null;
            }
            n2 = n2.next.next;
            n1 = n1.next;
        }
        n2 = head; // n2 -> walk again from head
        while (n1 != n2) {
            n1 = n1.next;
            n2 = n2.next;
        }
        return n1;
    }
		
}
```

## 终极判断  
```java
public class FindFirstIntersectNode {

    public static class Node {
        public int value;
        public Node next;

        public Node(int data) {
            this.value = data;
        }
    }

    public static Node findFirstIntersectNode(Node head1, Node head2) {
        if(head1 == null || head2 == null) {
            return null;
        }
        Node loopNode1 = getLoopNode(head1);
        Node loopNode2 = getLoopNode(head2);
        
        // no loop
        if(loopNode1 == null && loopNode2 == null) {
            return noLoop(head1, head2);
        }
        
        // before loopNode
        if(loopNode1 != null && loopNode2 != null) {
            return loopNode1;
        }
        
        return null;
    }

    public static Node getLoopNode(Node head) {
        if (head == null || head.next == null || head.next.next == null) {
            return null;
        }
        Node n1 = head.next; // n1 -> slow
        Node n2 = head.next.next; // n2 -> fast
        while (n1 != n2) {
            if (n2.next == null || n2.next.next == null) {
                return null;
            }
            n2 = n2.next.next;
            n1 = n1.next;
        }
        n2 = head; // n2 -> walk again from head
        while (n1 != n2) {
            n1 = n1.next;
            n2 = n2.next;
        }
        return n1;
    }

    public static Node noLoop(Node head1, Node head2) {
        Node cur1 = head1;
        Node cur2 = head2;
        int n = 0;
        while(cur1 != null) {
            cur1 = cur1.next;
            n++;
        }
        while(cur2 != null) {
            cur2 = cur2.next;
            n--;
        }
        if(cur1 != cur2) {
            return null;
        }
        cur1 = n > 0 ? head1 : head2;
        cur2 = cur1 == head1 ? head2 : head1;
        n = Math.abs(n);
        while(n > 0) {
            n--;
            cur1 = cur1.next;
        }
        while(cur1 != cur2) {
            cur1 = cur1.next;
            cur2 = cur2.next;
        }
        return cur1;
    }
        
    public static Node bothLoop(Node head1, Node loop1, Node head2, Node loop2) {
        Node cur1 = null;
        Node cur2 = null;
        if(loop1 == loop2) {
            cur1 = head1;
            cur2 = head2;
            int n = 0;
            while (cur1 != loop1) {
                n++;
                cur1 = cur1.next;
            }
            while (cur2 != loop2) {
                n--;
                cur2 = cur2.next;
            }
            cur1 = n > 0 ? head1 : head2;
            cur2 = cur1 == head1 ? head2 : head1;
            n = Math.abs(n);
            while (n != 0) {
                n--;
                cur1 = cur1.next;
            }
            while (cur1 != cur2) {
                cur1 = cur1.next;
                cur2 = cur2.next;
            }
            return cur1;
        } else {
            cur1 = loop1.next;
            while (cur1 != loop1) {
                if (cur1 == loop2) {
                    return loop1;
                }
                cur1 = cur1.next;
            }
            return null;
        }
        
    }
}
```