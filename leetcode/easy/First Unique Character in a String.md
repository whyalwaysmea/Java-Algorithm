## [Description](https://leetcode.com/problems/first-unique-character-in-a-string/#/description)  
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.  

**Examples:**  
```java
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```

## [Discuss](https://discuss.leetcode.com/category/509/first-unique-character-in-a-string)
**题意：**   
给出一个String， 找到第一个没有在该字符串中重复出现过的字符，如果不存在，则返回-1.  

**思考：**  
最简单的就是直接暴力了， 先遍历一次字符串，用一个Map将每个字符出现的次数保存下来。   
然后再来一次遍历，取出每个字符的出现次数， 如果该字符只出现过一次，那么就直接返回。   

**优化:**     
使用遍历，判断每个字符串第一次出现和最后一次出现的位置是否相同，如果相同则返回。   
但是这只是代码上的优化而已，它的复杂度是O(n^2),所以并没有什么实际上的优化。  



## Solution
**MySolution：**   
```java
public class Solution {
    public int firstUniqChar(String s) {
        int result = -1;
        HashMap<Character, Integer> charMap = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            Integer cTimes = charMap.get(Character.valueOf(c));
            if(cTimes == null) {
                charMap.put(Character.valueOf(c), 1);
            } else {
                charMap.put(Character.valueOf(c), ++cTimes);
            }
        }

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if(charMap.get(Character.valueOf(c)) == 1) {
                return i;
            }
        }

        return result;
    }
}
```

**Better Solution：**  
```java
public int firstUniqChar(String s) {
    for (int i = 0; i < s.length(); i++) {
        if (s.indexOf(s.charAt(i)) == s.lastIndexOf(s.charAt(i))) {
            return i;
        }
    }
    return -1;
}
```

```java
public class Solution {
    public int firstUniqChar(String s) {
        int freq [] = new int[26];
        for(int i = 0; i < s.length(); i ++)
            freq [s.charAt(i) - 'a'] ++;
        for(int i = 0; i < s.length(); i ++)
            if(freq [s.charAt(i) - 'a'] == 1)
                return i;
        return -1;
    }
}
```
