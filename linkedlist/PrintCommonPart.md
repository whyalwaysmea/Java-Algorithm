# 打印两个有序链表的公共部分  
【题目】给定两个有序链表的头指针head1和head2，打印两个链表的公共部分 

# 思考  
因为是有序的链表，所以可以用双指针的方式，两个指针一开始分别指向head1和head2，然后通过比较进行相应的移动  
如果head1等于head2，那么两个指针都指向下一个   
如果head1小于head2，那么head1指针指向下一个   
如果head1大于head2，那么head2指针指向下一个     

# 算法实现  
```java
public class PrintCommonPart {

    public static class Node {
        public int value;
        public Node next;

        public Node(int data) {
            this.value = data;
        }
    }

	public static void printCommonPart(Node head1, Node head2) {
		System.out.println("start print common part:");
		while(head1 != null && head2 != null) {
			if(head1.value == head2.value) {
				System.out.print(head1.value + " ");
				head1 = head1.next;
				head2 = head2.next;
			} else if(head1.value > head2.value) {
				head2 = head2.next;
			} else if(head1.value < head2.value) {
				head1 = head1.next;
			}
		}
	}
}
```