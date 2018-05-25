# 判断一个链表是否为回文结构 
【题目】给定一个单向链表的头节点head，请判断该链表是否为回文结构  
【要求】如果链表长度为N，时间复杂度达到O(N)，额外空间复杂度达到O(1)  
【示例】
>1 -> 2 -> 1 返回true   
1 -> 2 -> 2 -> 1 返回true   
1 -> 2 -> 3 返回false

# 思考 
回文的判断方式有很多种，这里先不考虑题目的要求，将常用的判断方法从简到繁过一遍  

## 借助额外数据结构（1）
在不考虑额外空间复杂度的情况下，可以借助一个外部的栈结构。   
首先遍历一遍链表，在遍历的过程中，将元素放入栈中   
然后再一次遍历链表，每次遍历的过程中，都将栈中的元素取出来进行比较，如果遍历过程中遇到不相等的情况，那么就不是回文结构了  

这种方法的额外空间复杂度是O(N) 


## 借助额外数据结构（2） 
可以对上一种方法进行一定的改进。  
因为是回文结构，所以只需要将前一半的数据和后一半的数据进行比较就可以了，这样额外空间复杂度就降低到了O(N/2)
1. 我们一开始用两个指针来进行遍历，A一次后移一步，B一次后移两步。当B无法移动了，那么A就来到了中间位置；    
2. 此时再让A继续移动，同时将数据放入栈中；   
3. 从头节点开始，比较节点和栈中的数据  
 
## 通过调整结构 
如果在不借助更多额外空间，那么我们可以通过改变链表的结构来进行判断。  
因为回文结构的特点，所以从头节点到中间节点和从尾节点到中间节点应该完全一样。 
我们可以将中间节点之后的链表都反向 
例如： 1 -> 2 -> 3 -> 2 -> 1 变成 1 -> 2 -> 3 <- 2 <- 1 
当然得让中间节点的next指向null，这样才能知道是不是到中间了。  

最后肯定还要将链表还原的了

# 实现 

## 借助额外数据结构（1）   
```java
public class IsPalindromeList {

	public static class Node {
		public int value;
		public Node next;

		public Node(int data) {
			this.value = data;
		}
	}
	
	public static boolean isPalindrome1(Node head) {
		Stack<Node> stack = new Stack<Node>();
		Node cur = head;
		while (cur != null) {
			stack.push(cur);
			cur = cur.next;
		}
		while (head != null) {
			if (head.value != stack.pop().value) {
				return false;
			}
			head = head.next;
		}
		return true;
	}
}
```

## 借助额外数据结构（2）  
```java
public class IsPalindromeList {

	public static class Node {
		public int value;
		public Node next;

		public Node(int data) {
			this.value = data;
		}
	}
	
	public static boolean isPalindrome2(Node head) {
		if(head == null || head.next == null) {
			return true;
		}
		Stack<Node> stack = new Stack<Node>();
		Node cur = head;
		Node end = head.next;
		while (end != null && end.next != null) {
			cur = cur.next;
			end = end.next.next;
		}
		while(cur.next != null) {
			stack.push(cur.next);
			cur.next = cur.next.next;
		}
		while(!stack.isEmpty()) {
			if(head.value != stack.pop().value) {
				return false;
			}
			head = head.next;
		}
		return true;
	}
}
```

## 通过调整结构 
```java
public static boolean isPalindrome3(Node head) {
    if (head == null || head.next == null) {
        return true;
    }
    Node n1 = head;
    Node n2 = head;
    while (n2.next != null && n2.next.next != null) { // find mid node
        n1 = n1.next; // n1 -> mid
        n2 = n2.next.next; // n2 -> end
    }
    n2 = n1.next; // n2 -> right part first node
    n1.next = null; // mid.next -> null
    Node n3 = null;
    while (n2 != null) { // right part convert
        n3 = n2.next; // n3 -> save next node
        n2.next = n1; // next of right node convert
        n1 = n2; // n1 move
        n2 = n3; // n2 move
    }
    n3 = n1; // n3 -> save last node
    n2 = head;// n2 -> left first node
    boolean res = true;
    while (n1 != null && n2 != null) { // check palindrome
        if (n1.value != n2.value) {
            res = false;
            break;
        }
        n1 = n1.next; // left to mid
        n2 = n2.next; // right to mid
    }
    n1 = n3.next;
    n3.next = null;
    while (n1 != null) { // recover list
        n2 = n1.next;
        n1.next = n3;
        n3 = n1;
        n1 = n2;
    }
    return res;
}
```