## [Description](https://leetcode.com/problems/longest-palindrome/description/) 

Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.  

This is case sensitive, for example "Aa" is not considered a palindrome here.   

**Note:**   
Assume the length of given string will not exceed 1,010.


**Example:**  
```
Input:
"abccccdd"

Output:
7

Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.
```
## [Discuss](https://discuss.leetcode.com/category/536/longest-palindrome)
**题意：**   
给定一个字符串，包含了大小写字母，找出可由这些字母所组成的最长的回文(palindrome)。

需要注意的是：这道题中要区分大小写，字符串长度不会超过1010  

**思考：**   
首先我们分析回文的规则： 可以看作是轴对称的，也就是说如果最后的结果是偶数的话，那么所有出现的字母都是成对出现的；如果是奇数的话，那么中间的那个字母是单个出现，其他的字母都是成对出现。  

现在我们的关键就在于：找出字符串中有多少个成对出现的字母，再找出这些成对字母后，再看看字符串中是否还有落单的，如果有那么就随便拧出来一个。    

所以问题就转换成了：计算字符串中有多少个成对的字母，同时除掉这些配对的字母后是否还有落单的字母。  
也就是说，直接计算有多少个配对的，然后观察配对的数量是否等于字符串的长度，如果不等于那么肯定就有落单的了。 


我们需要先遍历字符串的每一个字母，在遍历的过程中，当然就需要用一个数组来储存遍历过的字母了，这个数组应该是boolean类型的，用来判断是否有待配对的字母(该字母是否第一次出现)，如果配对成功后，则清除一下标志，下一个同样的字母再出现的时候，又当作第一次出现。   


**优化:**   
当然可以把数组换成HashSet之类的东西了 

## Solution
**MySolution：**   
```java
public int longestPalindrome(String s) {
    boolean[] seen = new boolean[128];
    int max = 0;
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        // 表明之前已经存了一个了
        if (seen[c]) {
            // 配对一组字母了，清空标志位
            seen[c] = false;
            // 结果长度+2
            max += 2;
        } else {
            // 表明待配对
            seen[c] = true;
        }
    }
    // 如果结果长度小于字符串长度，表明至少有一个待配对的，也就是说至少有一个落单的，所以+1
    // 结果等于字符串长度，表明所有的都配对成功了
    return max < s.length() ? max + 1 : max;
}
```

**Better Solution：**  
```java
public int longestPalindrome(String s) {
	if(s==null || s.length()==0) return 0;
    HashSet<Character> hs = new HashSet<Character>();
    int count = 0;
    for(int i=0; i<s.length(); i++){
        if(hs.contains(s.charAt(i))){
            hs.remove(s.charAt(i));
            count++;
        }else{
            hs.add(s.charAt(i));
        }
    }
    if(!hs.isEmpty()) return count*2+1;
    return count*2;
}
```