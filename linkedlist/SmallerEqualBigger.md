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

 