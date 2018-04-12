## [Description](https://leetcode.com/problems/base-7/description/)
Given an integer, return its base 7 string representation. 
**Example 1:**
>Input: 100
Output: "202"

**Example 2:**
>Input: -7
Output: "-10"

## [Discuss]()
**题意：**   
题意比较简单，就是把十进制转换成7进制，用string返回 

**思考：**   
这种偏数学性质的题目，先在草稿纸上演算一下，找到进制转换的规律。  
就拿100来举例 
1. 100 / 7 = 14 (余) 2  
2. 14 / 7 = 2 (余) 0 
3. 2 / 7 = 0 (余) 2

所以 100的7进制就是202

所以这里就可以联想到循环，用num一直除以7， 直到商等于0. 然后将这整个过程的余数整合起来

**另一种写法:**   
思路还是上面那样 
不过代码层面上可以改动一下， 换成用递归的方式来完成


## Solution
**MySolution：**   
```java
public class Solution {
    public String convertTo7(int num) {
        if (num == 0) return "0";
        
        StringBuilder sb = new StringBuilder();
        boolean negative = false;
        
        if (num < 0) {
            negative = true;
        }
        while (num != 0) {
            sb.append(Math.abs(num % 7));
            num = num / 7;
        }
        
        if (negative) {
            sb.append("-");
        }
        
        return sb.reverse().toString();
    }
}
```

**Another Solution：**  
```java
class Solution {
    public String convertToBase7(int num) {
        if (num < 0)
            return '-' + convertToBase7(-num);
        if (num < 7)
            return num + "";
        return convertToBase7(num / 7) + num % 7;
    }
}
```