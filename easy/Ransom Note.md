## [Description](https://leetcode.com/problems/ransom-note/#/description)
Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

**Note:**  
You may assume that both strings contain only lowercase letters.
```java
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```

## [Discuss](https://discuss.leetcode.com/category/503/ransom-note)
**题意：**   
给两个字符串，判断第一个字符串ransom能不能由第二个字符串magazines里面的字符构成，第二个字符里的每个字符只能使用一次。

**思考：**    
最容易想到的就是暴力做了，穷举法，两层遍历即可。  
第一层遍历先记录ransomNote每个字符出现的次数，  
第二层遍历记录magazine中每个字符出现的次数，  
最后对这两个次数记录进行比较。   

**优化:**   
其实目的就是比较两个字符串中每个字母的出现次数。     
对于某一个字母，如果它在ransomNote中的出现次数比magazine多，那么就返回false，反之则是true。

## Solution
**MySolution：**   
```java
public class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        if (ransomNote.equals(magazine) || "".equals(ransomNote)) {
            return true;
        }
        if ("".equals(magazine) && !"".equals(ransomNote)) {
            return false;
        }
        Map<Character, Integer> ransomNoteMap = new HashMap<>();
        Map<Character, Integer> magazineMap = new HashMap<>();
        int result = 0;
        for (int i = 0; i < ransomNote.length(); i++) {
            char temp = ransomNote.charAt(i);
            if(ransomNoteMap.containsKey(temp)) {
                Integer num = ransomNoteMap.get(temp);
                ransomNoteMap.put(temp, ++num);
            } else {
                ransomNoteMap.put(temp, 1);
            }
        }

        for (int i = 0; i < magazine.length(); i++) {
            char temp = magazine.charAt(i);
            if(magazineMap.containsKey(temp)) {
                Integer num = magazineMap.get(temp);
                magazineMap.put(temp, ++num);
            } else {
                magazineMap.put(temp, 1);
            }
        }
        for (Map.Entry<Character, Integer> entries : ransomNoteMap.entrySet()) {
            Character key = entries.getKey();
            Integer num = entries.getValue();
            if(!magazineMap.containsKey(key)) {
                result = 0;
                return false;
            }
            if(num <= magazineMap.get(key)) {
                result++;
            }
        }
        if(result == ransomNoteMap.size() && result != 0) {
            return true;
        }
        return false;
    }
}
```

**Better Solution：**  
```java
public class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] arr = new int[26];
        // 记录magazine中每个字母的出现次数
        for (int i = 0; i < magazine.length(); i++) {
            arr[magazine.charAt(i) - 'a']++;
        }
        // 获取ransomNote中每个字母的出现次数
        // 然后和magazine中的该字母出现的次数比较
        for (int i = 0; i < ransomNote.length(); i++) {
            if(--arr[ransomNote.charAt(i)-'a'] < 0) {
                return false;
            }
        }
        return true;
    }
}
```