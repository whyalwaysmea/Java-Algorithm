# 将单向链表按某值划分成左边小、中间相等、右边大的形式   
【题目】给定一个单向链表的头节点head，节点的值类型是整型，再给定一个整数pivot。实现一个调整链表的函数，将链表调整为左部分都是小于pivot的节点，中间部分都是值等于pivot的节点，右边部分都是值大于pivot的节点。对于调整后的节点顺序没有更多的要求      

【示例】    
链表：9 -> 0 -> 4 -> 5 -> 1，pivot = 3。   
调整后的链表可以是：0 -> 1 -> 9 -> 4 -> 5；也可以是：0 -> 1 -> 9 -> 5 ->4   


# 思考 
其实就是排序中的[荷兰国旗问题](https://github.com/whyalwaysmea/Java-Algorithm/blob/master/sort/NetherlandsSort.md)，所以我们直接将链表转换成数组，进行Partition，最后又转成链表就可以了  

# 实现 
```java
public class SmallerEqualBigger {

    public static class Node {
        public int value;
        public Node next;

        public Node(int data) {
            this.value = data;
        }
    }

    public static Node listPartition1(Node head, int pivot) {
        if (head == null) {
            return head;
        }
        Node cur = head;
        int i = 0;
        while (cur != null) {
            i++;
            cur = cur.next;
        }
        Node[] nodeArr = new Node[i];
        i = 0;
        cur = head;
        for (i = 0; i != nodeArr.length; i++) {
            nodeArr[i] = cur;
            cur = cur.next;
        }
        arrPartition(nodeArr, pivot);
        for (i = 1; i != nodeArr.length; i++) {
            nodeArr[i - 1].next = nodeArr[i];
        }
        nodeArr[i - 1].next = null;
        return nodeArr[0];
    }

    public static void arrPartition(Node[] nodeArr, int pivot) {
        int small = -1;
        int big = nodeArr.length;
        int index = 0;
        while (index != big) {
            if (nodeArr[index].value < pivot) {
                swap(nodeArr, ++small, index++);
            } else if (nodeArr[index].value == pivot) {
                index++;
            } else {
                swap(nodeArr, --big, index);
            }
        }
    }

    public static void swap(Node[] nodeArr, int a, int b) {
        Node tmp = nodeArr[a];
        nodeArr[a] = nodeArr[b];
        nodeArr[b] = tmp;
    }
}    
```   

 # 进阶 
 在原问题的要求上再增加如下两个要求:  
 1. 在左、中、右三个部分的内部做顺序要求，要求每部分里的节点从左到右的顺序与原链表中节点的先后次序一次  
 2. 如果链表长度为N，时间复杂度请达到O(N)，额外空间复杂度为O(1) 

 【示例】  
 链表：9 -> 0 -> 4 -> 5 -> 1，pivot = 3。    
 调整后的链表：0 -> 1 -> 9 -> -> 4 -> 5  

 # 思考  
原有的方式有如下不足： 
1. partition的过程不是稳定的，会导致顺序错乱  
2. 额外空间复杂度为O(N)  

考虑到链表的特性，以及最后结果有一定的分区性，所以我们可以将结果看作是三个链表组合起来的  
小于pivot的为一部分，等于的为一部分，大于的是一部分，最后再将这三个拼接起来就好了  
对于每一部分而言，我们一开始先找到头节点，也就是原链表中第一个小于、等于、大于pivot的节点，然后再遍历原链表，将每个节点放进对应的部分就好了      


 # 实现 
 ```java
public class SmallerEqualBigger {

	public static class Node {
        public int value;
        public Node next;

        public Node(int data) {
            this.value = data;
        }
    }

	public static Node listPartition1(Node head, int pivot) {
		Node sH = null; 	// small head
		Node sT = null; 	// small tail
		Node eH = null; 	// equal head
		Node eT = null; 	// equal tail
		Node bH = null; 	// big head
		Node bT = null; 	// big tail
		Node next = null; 	// save next node
		// every node distributed to three lists
		while (head != null) {
			next = head.next;
			head.next = null;
			if (head.value < pivot) {
				if (sH == null) {
					sH = head;
					sT = head;
				} else {
					sT.next = head;
					sT = head;
				}
			} else if (head.value == pivot) {
				if (eH == null) {
					eH = head;
					eT = head;
				} else {
					eT.next = head;
					eT = head;
				}
			} else {
				if (bH == null) {
					bH = head;
					bT = head;
				} else {
					bT.next = head;
					bT = head;
				}
			}
			head = next;
		}
		// small and equal reconnect
		if (sT != null) {
			sT.next = eH;
			eT = eT == null ? sT : eT;
		}
		// all reconnect
		if (eT != null) {
			eT.next = bH;
		}
		return sH != null ? sH : eH != null ? eH : bH;
	}
}
 ```