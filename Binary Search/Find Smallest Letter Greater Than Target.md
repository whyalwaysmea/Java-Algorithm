## [Description](https://leetcode.com/problems/find-smallest-letter-greater-than-target/description/)
Given a list of sorted characters letters containing only lowercase letters, and given a target letter target, find the smallest element in the list that is larger than the given target.

Letters also wrap around. For example, if the target is target = 'z' and letters = ['a', 'b'], the answer is 'a'.

Examples:
```
Input:
letters = ["c", "f", "j"]
target = "a"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "c"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "d"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "g"
Output: "j"

Input:
letters = ["c", "f", "j"]
target = "j"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "k"
Output: "c"
```

**Note:** 
1. letters has a length in range [2, 10000].
2. letters consists of lowercase letters, and contains at least 2 unique letters.
3. target is a lowercase letter.


## [Discuss]()
**题意：**   
给定一个排序好的小写字符数组，同时给出一个目标的小写字符，从数组中找出那个最小的刚好大于目标字符的字符。 
给定的数组应该是循环的，比如数组是['a','b','c'],给定的目标字符是'z'，那么答案就是'a' 

需要注意的是： 
字符数组中可能存在重复的字符

**思考：**  

**优化:**   


## Solution
**MySolution：**   
```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        for(int i = 0; i < letters.length; i++) {
            if(letters[i] - target > 0 ) {
                return letters[i];
            }
        }
        return letters[0];
    }
}
```
时间复杂度： O(N)
空间复杂度： O(1)

**Better Solution：**  
```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int lo = 0, hi = letters.length;
        while (lo < hi) {
            int mi = lo + (hi - lo) / 2;
            if (letters[mi] <= target) lo = mi + 1;
            else hi = mi;
        }
        return letters[lo % letters.length];
    }
}
```
时间复杂度： O(logN)
空间复杂度： O(1)