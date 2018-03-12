## [Description](https://leetcode.com/problems/pascals-triangle-ii/description/)
Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,
Return `[1,3,3,1]`

**Note:**
Could you optimize your algorithm to use only O(k) extra space?

## [Discuss]()
**题意：**   
首先我们需要了解一下什么是[Pascal's Triangle](https://en.wikipedia.org/wiki/Pascal%27s_triangle)

![Pascal's triangle](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/PascalTriangleAnimated2.gif/220px-PascalTriangleAnimated2.gif)

能够理解到Pascal's Triangle的原理之后，题意就很清晰了。 
给定一个序列k，我们就返回Pascal's Triangle的第k行。 当然这个是从第0行开始的
**思考：**   
因为目前知道的是第0行的数据，所以想要知道第k行的数据，那么可以倒推回去。先计算0，1，2... 直到第k行

**优化:**   
方法1是通过递归来实现的。  其实也可以通过循环，从第0层开始一层一层的计算。 
这里我们可以巧妙的利用ArrayList的两个方法
```java
add(int index, E element)	// 添加元素，区别于一般的add(E e)，这个就是有个位置的概念，特殊位置之后的数据，依次往后移动就是了。
set(int index, E element)	// 更新元素，更新指定下标位置的值。
```

## Solution
**MySolution：**   
```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> result = new ArrayList<Integer>();
		if(rowIndex == 0) {
			result.add(1);
			return result;
		} 
		List<Integer> temp = getRow(rowIndex - 1);
		result.add(1);
		for (int i = 1; i < temp.size(); i++) {
			int a = temp.get(i - 1);
			int b = temp.get(i);
			result.add(a + b);
		}
		result.add(1);
		return result;
    }
}
```

**Better Solution：**  
```java
  public List<Integer> getRow(int rowIndex) {
	List<Integer> list = new ArrayList<Integer>();
	if (rowIndex < 0)
		return list;

	for (int i = 0; i < rowIndex + 1; i++) {
		list.add(0, 1);
		for (int j = 1; j < list.size() - 1; j++) {
			list.set(j, list.get(j) + list.get(j + 1));
		}
	}
	return list;
}
```