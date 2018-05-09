# 用数组结构实现大小固定的栈 
栈是先进后出，后进后出的  

用数组结构实现大小固定的栈，包含以下基本方法： 
- 压栈操作push  
- 弹栈操作pop  
- 查看栈顶元素peek  
- 查看栈的大小  
- 查看栈是否为空  

基本思路：用一个变量来维护数组的实际大小，它所指的位置就是下一个元素进来需要存放的位置

```java
public class ArrayToStack {
    private int start;
    private int end;
    private int size;
    
    private Integer[] arr;
    
    public ArrayToStack(int size) {
        arr = new Integer[size];
    }
    
    /**
     * 压栈
     * @param obj
     */
    public void push(int obj) {
        if(size == arr.length) {
            throw new ArrayIndexOutOfBoundsException("The Stack if full");
        }
        arr[size++] = obj;
    }
    
    /**
     * 出栈
     * @return
     */
    public Integer pop() {
        if(size == 0) {
            throw new ArrayIndexOutOfBoundsException("The Stack if empty");
        }
        return arr[--size];
    }
    
    /**
     * @return 栈顶元素
     */
    public Integer peek() {
        if(size == 0) {
            throw new ArrayIndexOutOfBoundsException("The Stack if empty");
        }
        return arr[size-1];
    }
    
    /**
     * @return 栈的大小
     */
    public Integer size() {
        return size;
    }

    /**
     * @return	栈是否为空
     */
    public boolean isEmpty() {
        return size == 0;
    }
}
```

# 用数组结构实现大小固定的队列 
队列是先进先出，后进后出的 

用数组结构实现大小固定的队列，包含以下基本方法： 
- 入队操作put  
- 出队操作pop  
- 查看队头元素peek  
- 查看队列的大小  
- 查看队列是否为空  

基本思路：用一个start，一个end来分别表示队列的头和尾，再用一个size来表示当前队列大小。start和end都是可以循环的，配合起来可以把数组循环使用 

```java
public class ArrayToQueue {
	private int start;
	private int end;
	private int size;
	private Integer[] arr;
	public ArrayToQueue(int size) {
		arr = new Integer[size];
	}
	
	/**
	 * 入队
	 * @param obj
	 */
	public void put(int obj) {
		if(size == arr.length) {
			throw new ArrayIndexOutOfBoundsException("The Queue if full");
		}
		arr[end] = obj;
		size++;
        // 判断end是否超过范围，如果是重新调整
		end = end == arr.length - 1 ? 0 : end + 1;
	}
	
	/**
	 * @return 出队
	 */
	public int pop() {
		if(size == 0) {
			throw new ArrayIndexOutOfBoundsException("The Queue if empty");
		}
		size--;
		int result = arr[start];
		start = start == arr.length - 1 ? 0 : start + 1; 
		return result;
	}
	
	/**
	 * @return 头部元素
	 */
	public int peek() {
		if(size == 0) {
			throw new ArrayIndexOutOfBoundsException("The Queue if empty");
		}
		return arr[start];
	}
}
```