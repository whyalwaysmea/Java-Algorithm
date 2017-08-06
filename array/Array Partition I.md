## [Description](https://leetcode.com/problems/array-partition-i/#/description)
Given an array of 2n integers, your task is to group these integers into n pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible.

**Example 1:**   
```java
Input: [1,4,3,2]     

Output: 4       
Explanation: n is 2, and the maximum sum of pairs is 4.
```

**Note:**
1. n is a positive integer, which is in the range of [1, 10000].
2. All the integers in the array will be in the range of [-10000, 10000].

## [Discuss](https://discuss.leetcode.com/category/718/array-partition-i)
给定一个长度为2n(偶数)的数组，分成n个小组，然后将每个组中较小的那个数相加，然后返回这个和。  

所以其实就是升序的排序，然后将偶数位的数字相加起来。
一开始我排序用了快排，结果比较耗时，其实可以直接用`Arrays.sort`对数组进行排序的

## Solution
```java
public class Solution {
    public int arrayPairSum(int[] nums) {
        Arrays.sort(nums);

        int result = 0;
        for (int i = 0; i < nums.length; i+=2) {
            result += nums[i];
        }
        return result;
    }
}
```
