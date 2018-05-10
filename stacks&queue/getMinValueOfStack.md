# 获取栈中最小元素  
实现一个特殊的栈，在实现栈的基本功能的基础上，再实现返回栈中最小元素的操作  

【要求：】 
1. pop、push、getMin操作的时间复杂度都是O(1)  
2. 设计的栈类型可以使用现场的栈结构  


# 思考 
因为可以使用现成的栈结果，所以我们可以先使用一个栈stackData来保存数据，用它来实现基本的功能。
现在需要考虑的就是getMin的实现，因为只是返回栈中最小的元素并不是出栈，所以我们可以再用一个栈stackMin来保存最小元素。 
stackMin所保存的是相对的最小元素，每次有元素添加进stackData的时候，先与stackMin栈顶元素比较，如果新元素更小，那么就将它也加入stackMin。出栈的时候，先将stackData中要出栈的元素与stackMin进行比较，如果是同一个元素，那么就将stackMin的栈顶元素也出栈。  


# 实现  
```java
public static class MyStack1 {
    private Stack<Integer> stackData;
    private Stack<Integer> stackMin;

    public MyStack1() {
        this.stackData = new Stack<Integer>();
        this.stackMin = new Stack<Integer>();
    }

    public void push(int newNum) {
        if (this.stackMin.isEmpty()) {
            this.stackMin.push(newNum);
        } else if (newNum <= this.getmin()) {
            this.stackMin.push(newNum);
        }
        this.stackData.push(newNum);
    }

    public int pop() {
        if (this.stackData.isEmpty()) {
            throw new RuntimeException("Your stack is empty.");
        }
        int value = this.stackData.pop();
        if (value == this.getmin()) {
            this.stackMin.pop();
        }
        return value;
    }

    public int getmin() {
        if (this.stackMin.isEmpty()) {
            throw new RuntimeException("Your stack is empty.");
        }
        return this.stackMin.peek();
    }
}

public static class MyStack2 {
    private Stack<Integer> stackData;
    private Stack<Integer> stackMin;

    public MyStack2() {
        this.stackData = new Stack<Integer>();
        this.stackMin = new Stack<Integer>();
    }

    public void push(int newNum) {
        if (this.stackMin.isEmpty()) {
            this.stackMin.push(newNum);
        } else if (newNum < this.getmin()) {
            this.stackMin.push(newNum);
        } else {
            int newMin = this.stackMin.peek();
            this.stackMin.push(newMin);
        }
        this.stackData.push(newNum);
    }

    public int pop() {
        if (this.stackData.isEmpty()) {
            throw new RuntimeException("Your stack is empty.");
        }
        this.stackMin.pop();
        return this.stackData.pop();
    }

    public int getmin() {
        if (this.stackMin.isEmpty()) {
            throw new RuntimeException("Your stack is empty.");
        }
        return this.stackMin.peek();
    }
}
```