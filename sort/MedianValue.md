# 求中值 
中值就是排序后中间那个元素的值，如果元素个数为奇数，中值是没有歧义的，如果是偶数，可以为偏小的，也可以为偏大的。  

一个简单的思路就是排序，排序后取中间的那个值就可以了。排序可以使用Arrays.sort()方法，效率为O(N*log(N))。 
当然，这是要在数据源已知的情况下才能做到的。如果数据源源不断到来呢？ 

可以使用两个堆，一个最大堆，一个最小堆 
1. 假设当前的中位数为m，最大堆维护的是<=m的元素，最小堆维护的是>=m的元素，但两个堆都不包含m。 
2. 当新的元素到达时，比如为e，将e与m进行比较，若e<=m，则将其加入最大堆中，否则加入最小堆中 
3. 如果此时最小堆和最大堆的元素个数相差>=2，则将m加入元素个数少的堆中，然后从元素个数多的堆将根节点移除并赋值给m。 

给个示例解释一下，输入的元素依次是：34，90，67，45，1 
1. 输入第一个元素时，m赋值为34 
2. 输入第二个元素时，90>34，把90加入最小堆，m不变 
3. 输入第三个元素时，67>34，把67加入最小堆，此时最小堆根节点为67。但是现在最小堆中元素个数为2，最大堆中元素个数为0，所以需要做调整，把m(34)加入个数少的堆中（最大堆），然后从元素个数多的堆（最小堆）将根节点移除并赋值给m，所以现在m为67，最大堆中有34，最小堆中有90 
4. 输入第四个元素时，45<67，把45加入最大堆，m不变 
5. 输入第五个元素时，1<67，把1加入最大堆中，此时m为67，最大堆中有1，34，45，最小堆中有90，所以需要调整。调整后，m为45，最大堆为1，34，最小堆为67，90。 


# 算法实现  
```java
public class Median<E> {
    // 最小堆
    private PriorityQueue<E> minP;
    // 最大堆
    private PriorityQueue<E> maxP;
    // 中值
    private E m;
    public Median() {
        this.minP = new PriorityQueue<>();
        this.maxP = new PriorityQueue<>(11, Collections.reverseOrder());
    }
    // 比较
    private int compare(E e, E m) {
        Comparable<? super E> cmpr = (Comparable<? super E>)e;
        return cmpr.compareTo(e);
    }
    public void add(E e) {
        if(m == null) {
            // 第一个元素
            m = e;
            return ;
        }        
        if(compare(e, m) < 0) {
            // 如果e小于m，则加入最大堆
            maxP.add(e);
        } else {
            // 如果e大于m，则加入最小堆
            minP.add(e);
        }
        if(minP.size() - maxP.size() >= 2) {
            // 最小堆中元素比最大堆中元素多2个
            // 将m加入最大堆中，然后将最小堆中的根移除赋值给m
            maxP.add(this.m);
            this.m = minP.poll();
        } else if(maxP.size() - minP.size() >= 2) {
            minP.add(this.m);
            this.m = maxP.poll();
        }
    }
    public void addAll(Collection<? extends E> c) {
        for(E e : c) {
            add(e);
        }
    }
    public E getM() {
        return m;
    }
}
```