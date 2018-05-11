# 如何仅用队列结构实现栈结构?
## 思考 
主要的考察点就是出栈的这个方法 
单纯的用一个队列是无法满足栈结构的，所以我们可以考虑使用两个队列结构  
第一个队列queueData用来存储数据，第二个队列queueHelp做中转输出。 
中转输出的意思就是，在需要peek或者pop元素的时候，先将queueData中的元素挨着出队，放入queueHelp队列中，直到剩下最后一个元素，那么最后这个元素就是需要返回给调用方的。
再完成中转输出之后，queueData就空了，queueHelp就是剩下的元素了，此时只需要改变一下两者的引用就可以了

## 实现
```java
public static class TwoQueueStack {
	private Queue<Integer> queueData;
	private Queue<Integer> queueHelp;
	
	public TwoQueueStack() {
		queueData = new LinkedList<Integer>();
		queueHelp = new LinkedList<Integer>();
	}
	
	public void push(Integer newItem) {
		queueData.add(newItem);
	}
	
	public Integer peek() {
		if(queueData.isEmpty()) {
			throw new ArrayIndexOutOfBoundsException("The Stack is empty");
		}
        // 中转数据，剩最后一个
		while(queueData.size() != 1) {
			queueHelp.add(queueData.poll());
		}
		Integer result = queueData.poll();
		queueHelp.add(result);
		swap();
		return result;
	}
	
	public Integer pop() {
		if(queueData.isEmpty()) {
			throw new ArrayIndexOutOfBoundsException("The Stack is empty");
		}
        // 中转数据，剩最后一个
		while(queueData.size() != 1) {
			queueHelp.add(queueData.poll());
		}
		Integer result = queueData.poll();
		swap();
		return result;
	}
	
    // 交换引用
	private void swap() {
		Queue<Integer> tmp = queueHelp;
		queueHelp = queueData;
		queueData = tmp;
	}
}
```

# 如何仅用栈结构实现队列结构? 
## 思考 
同样的道理，我们应该使用两个栈来配合 
因为栈是先进后出，所以我们可以用两个栈来相互倒腾数据  

这里以poll为例：
如果一开始数据进入栈A，进入了1，2，3。 现在要出队了，应该出1。我们将栈A的数据倒腾进栈B，那么栈B的数据就是：3，2，1。此时栈B的数据出一个，就是正常的出队了。  
此时栈A是空，栈B是3，2。 当再有数据进来也放入栈A，要出数据的时候，因为栈B中有数据，所以不用倒腾。只有当栈B中没有数据的时候，才需要倒腾一下

```java
public static class TwoStacksQueue {
    private Stack<Integer> stackPush;
    private Stack<Integer> stackPop;

    public TwoStacksQueue() {
        stackPush = new Stack<Integer>();
        stackPop = new Stack<Integer>();
    }

    public void push(int pushInt) {
        stackPush.push(pushInt);
    }

    public int poll() {
        if (stackPop.empty() && stackPush.empty()) {
            throw new RuntimeException("Queue is empty!");
        } else if (stackPop.empty()) {
            while (!stackPush.empty()) {
                stackPop.push(stackPush.pop());
            }
        }
        return stackPop.pop();
    }

    public int peek() {
        if (stackPop.empty() && stackPush.empty()) {
            throw new RuntimeException("Queue is empty!");
        } else if (stackPop.empty()) {
            while (!stackPush.empty()) {
                stackPop.push(stackPush.pop());
            }
        }
        return stackPop.peek();
    }
}
```