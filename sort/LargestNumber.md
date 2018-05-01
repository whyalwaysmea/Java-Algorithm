# 求前K个最大的元素  
求前K个最大的元素，元素个数不确定，数据量可能很大，甚至源源不断到来，但需要知道目前为止最大的前K个元素   


# 思路  
维护一个长度为K的数组，数组中的元素就是目前最大的K个元素，以后每来一个新元素的时候，都先找到数组中最小值，将新元素与最小值相比，如果小于最小值，什么都不用做；如果大于最小值，则将最小值替换为新元素。 

使用最小堆维护这个K个元素，最小堆中，根即第一个元素永远都是最小的，新来的元素与根比较就可以了，如果小于根，则堆不需要变化，否则用新元素替换根，然后向下调整堆即可，调整的效率为O(logK)，总体效率就是O(N*logK)。而且使用了最小堆后，第K个最大的元素也很容易获取，它就是堆的根 

# 实现  
```java
public class TopK<E> {
    // 最小堆
    private PriorityQueue<E> p;
    private int k;
    public TopK(int k) {
        this.k = k;
        this.p = new PriorityQueue<>(k);
    }
    public void addAll(Collection<? extends E> c) {
        for(E e: c) {
            add(e);
        }
    }
    public void add(E e) {
        // 如果当前堆内元素小于K，则直接放入
        if(p.size() < k) {
            p.add(e);
            return ;
        }
        // 取出堆的头元素，也就是最小值
        Comparable<? super E> head = (Comparable<? super E>)p.peek();
        if(head.compareTo(e)>0) {
            // 小于TopK中的最小值，不用变
            return ;
        }
        // 新元素替换掉原来最小值成为TopK之一
        p.poll();
        p.add(e);
    }
    public <T> T[] toArray(T[] a) {
        return p.toArray(a);
    }
    public E getKth() {
        return p.peek();
    }
}
```