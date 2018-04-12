## [Description](https://leetcode.com/problems/count-and-say/description/)  
The count-and-say sequence is the sequence of integers with the first five terms as following:  
1.     1
2.     11
3.     21
4.     1211
5.     111221

`1` is read off as `"one 1"` or `11`.
`11` is read off as `"two 1s"` or `21`.
`21` is read off as `"one 2, then one 1"` or `1211`.

Given an integer n, generate the nth term of the count-and-say sequence.

Note: Each term of the sequence of integers will be represented as a string.

**Example1: **
>Input: 1
Output: "1"

**Example2:**
>Input: 4
Output: "1211"

## [Discuss]()
**题意：**   
大意就是统计并读出数字。  
比如
1： "1"。  
2： "11" 。  一个1 
3： "21"。  两个1 
4： “1211”  一个2一个1   

**思考：**   
因为知道1 对应的就是“1”。 所以就想到了使用递归的方式倒推回来，  
知道了1，就可以统计出2应该是多少，因为只需要遍历一下就可以的了 

**优化:**     
目前还没有想到更好的思路，大多是从编码层面来优化的 


## Solution
**MySolution：**   
```java
class Solution {
    public String countAndSay(int n) {
        if(n == 1) {
			return "1"; 
		}
		// 递归一下
		String tempStr = countAndSay(n - 1);
		// 统计数字出现的次数
		List<String> nums = new ArrayList<String>();
		// 统计所出现的数字
		List<String> values = new ArrayList<String>();
		// 记录当前第一个数字字符
		String current = tempStr.charAt(0) + "";
		// 因为记录了第一个数字了，那么该数字就出现了一次了
		int currentNum = 1;
		// 开始遍历字符串 		
		for (int i = 1; i < tempStr.length(); i++) {
			// 取出当前遍历的字符
			String temp = tempStr.charAt(i) + "";
			// 判断和记录的数字字符是否相等，如果相当计数+1
			if(current.equals(temp)) {
				currentNum++;
			} else {
				// 如果不相等，将数字字符和计数统计起来，标志位复原
				nums.add(currentNum + "");
				values.add(current);
				current = temp;
				currentNum = 1;
			}
		}
		// 将最后一个数字字符统计起来
		nums.add(currentNum + "");
		values.add(current);
		
		// 根据统计的数据 “读”出来
		StringBuilder result = new StringBuilder();
		for (int i = 0; i < nums.size(); i++) {
			result.append(nums.get(i));
			result.append(values.get(i));
		}
		
		return result.toString();
    }
}
```

**Better Solution：**  
```java
public String countAndSay(int n) {
    String ret = ""+1;
    
    while(--n  > 0)
        ret = apply(ret);
    
    return ret;
}

String apply(String s){
    StringBuilder ret = new StringBuilder();
    
    for(int i = 0, count =0; i  < s.length() ; ){
        while(i + count < s.length() && s.charAt(i) == s.charAt(i + count))
            count ++;
                
        ret.append(count).append(s.charAt(i));
        i += count; 
        count = 0;
    }
    
    return ret.toString();
}
```