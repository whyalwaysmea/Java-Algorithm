# 复制含有随机指针节点的链表 
【题目】一种特殊的链表节点类描述如下：  
```java
public class Node {
    public int value;
    public Node next;
    public Node rand;
    public Node(int data) {
        this.value = data;
    }
}
``` 
Node类中的value是节点值，next指针和正常单向链表中的next指针意义一样，都指向下一个节点，rand指针是Node类中新增的指针，这个指针可能指向链表中的任意一个节点，也可能指向null。  
给定一个由Node节点类型组成的无环单向链表的头节点head，请实现一个函数完成这个链表中所有结构的复制，并返回复制的新链表的头节点。 

【进阶】不实用额外的数据结构，只用有限几个变量，且时间复杂度为O(N) 

# 思考  
首先我们借助额外的数据结构来完成功能  
因为有复制的这个过程，所以可以想到hash结构，旧值为key，新值为value，这样挨着遍历拼接就好了


# 实现 
```java
public class CopyListWithRandom {

	public static class Node {
	    public int value;
	    public Node next;
	    public Node rand;
	    public Node(int data) {
	        this.value = data;
	    }
	}
	
	public static Node CopyListWithRandom(Node head) {
		HashMap<Node, Node> map = new HashMap<Node, Node>();
		Node cur = head;
		while (cur != null) {
			map.put(cur, new Node(cur.value));
			cur = cur.next;
		}
		cur = head;
		while (cur != null) {
			map.get(cur).next = map.get(cur.next);
			map.get(cur).rand = map.get(cur.rand);
			cur = cur.next;
		}
		return map.get(head);
	}
}
```