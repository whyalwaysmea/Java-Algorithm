## [Description](https://leetcode.com/problems/excel-sheet-column-title/#/description)
Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:    
![Markdown](http://i4.piimg.com/1949/11e8924ff90a85a9.png)


## [Discuss]()
**题意：**   
类似与26进制的转换，只不过是从1开始的     

**思考：**  
一开始可以根据Demo里的规则来进行推断，
我们先假设0 < n <= 26. 那么对应的字符串应该是 "A" + (n - 1) % 26；    
那么如果n > 26了呢。 我们只用先按照上面的方法操作， 然后 n = (n-1)/26，最后再继续上面的操作;     
这样就一位一位的进行转换， 先得到的是低位，后得到的是高位， 可以用StringBuilder.insert()方法   


**优化:**   
之前的解法是用的循环来做的，其实可以用递归。   
思路和上面大体差不多，只是写法上的一些区别。   



## Solution
**MySolution：**   
```java
public class Solution {
    public String convertToTitle(int n) {
        StringBuilder result = new StringBuilder();

        while(n>0){
            n--;
            result.insert(0, (char)('A' + n % 26));
            n /= 26;
        }

        return result.toString();
    }
}
```

**Better Solution：**  
```java
public class Solution {
    public String convertToTitle(int n) {
        return n == 0 ? "" : convertToTitle(--n / 26) + (char)('A' + (n % 26));
    }
}
```
