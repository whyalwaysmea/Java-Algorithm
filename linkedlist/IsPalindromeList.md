# 判断一个链表是否为回文结构 
【题目】给定一个单向链表的头节点head，请判断该链表是否为回文结构  
【要求】如果链表长度为N，时间复杂度达到O(N)，额外空间复杂度达到O(1)  
【示例】
>1 -> 2 -> 1 返回true   
1 -> 2 -> 2 -> 1 返回true   
1 -> 2 -> 3 返回false

# 思考 
回文的判断方式有很多种，这里先不考虑题目的要求，将常用的判断方法从简到繁过一遍  

## 借助额外数据结构  
在不考虑额外空间复杂度的情况下，可以借助一个外部的栈结构。   
首先遍历一遍链表，在遍历的过程中，将元素放入栈中   
然后再一次遍历链表，每次遍历的过程中，都将栈中的元素取出来进行比较，如果遍历过程中遇到不相等的情况，那么就不是回文结构了  

这种方法的额外空间复杂度是O(N) 


# 实现 

## 借助额外数据结构   
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