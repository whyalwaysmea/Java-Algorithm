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

其次再考虑不借助额外数据结构的情况  
我们可以考虑对原有的结构做一定调整  

我们先通过一次遍历，产生每一个节点的复制节点，放在原节点的后面，如下：   
原链表： 1 -> 3 -> 2  
复制后： 1 -> 1' -> 3 -> 3' -> 2 -> 2' 
这里带有'就表示复制节点  

这样通过原节点，就很容易的可以找到该节点的复制节点   
比如 head.rand 的复制节点就是 head.rand.next  
那么我们再来调整随机指针的节点  

最后将链表进行拆分


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

```java
public static Node CopyListWithRandom2(Node head) {
    if (head == null) {
        return null;
    }
    Node cur = head;
    Node next = null;
    // copy node and link to every node
    while (cur != null) {
        next = cur.next;
        cur.next = new Node(cur.value);
        cur.next.next = next;
        cur = next;
    }
    cur = head;
    Node curCopy = null;
    // set copy node rand
    while (cur != null) {
        next = cur.next.next;
        curCopy = cur.next;
        curCopy.rand = cur.rand != null ? cur.rand.next : null;
        cur = next;
    }
    Node res = head.next;
    cur = head;
    // split
    while (cur != null) {
        next = cur.next.next;
        curCopy = cur.next;
        cur.next = next;
        curCopy.next = next != null ? next.next : null;
        cur = next;
    }
    return res;
}
```