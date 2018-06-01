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