## [Description](https://leetcode.com/problems/valid-perfect-square/description/)
Given a positive integer num, write a function which returns True if num is a perfect square else False.

Note: Do not use any built-in library function such as sqrt.

**Example 1:** 
>Input: 16
Returns: True

**Example 2:**
>Input: 14
Returns: False

## [Discuss]()
**题意：**   
题目还是比较简单的，就是给定一个正整数，然后判断它是否是一个完全平方数。 
当然，肯定是不能直接使用第三方库，或者自带的`sqrt`方法。

**思考：**   
比较容易想到的就是从1开始循环，看看是否能够得到一个i，i²=num 

当然如果给定的num值很大，从1开始遍历循环肯定是不可取的，因为太耗时了。

所以我们可以联想到[二分查找](https://github.com/whyalwaysmea/LearningNotes/blob/master/Algorithm/%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.md)的方式

**优化:**   
因为求一个数是否是完全平方数，那么可以根据一个数学公式得来

1+3+5+7+...,

## Solution
**MySolution：**   
```java
public boolean isPerfectSquare(int num) {
    // 这里用long比较好，如果用int可能会导致int数值超出范围
    long low = 0;
    long high = num;
    long middle = 0;
    while(low <= high) {
        middle = (low + high) / 2;
        long result = middle * middle;
        if(result == num) {
            return true;
        } else if(result < num) {
            low = middle + 1;
        } else {
            high = middle - 1;
        }
    }

    return false;
}
```
时间复杂度是:O(log(n))

**Better Solution：**  
```java
public static boolean square(int num) {
    int i = 1;
    while (num > 0) {
         num = num - i;
         i = i + 2;
     }
     return num == 0;
}
```
时间复杂度是 O(sqrt(n)),