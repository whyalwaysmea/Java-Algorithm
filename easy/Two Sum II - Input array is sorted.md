## [Description](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/#/description)
Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution and you may not use the same element twice.  

**Input:** numbers={2, 7, 11, 15}, target=9   
**Output:** index1=1, index2=2

## [Discuss](https://discuss.leetcode.com/category/175/two-sum-ii-input-array-is-sorted)
**题意：** 给出一个升序排序的数组，然后从数组中找出两个数，它们的和等于指定的数。最后返回这两个数的角标，index1必须小于index2，同时需要注意角标不是从0开始的。

**思考：**
一开始的想法是：直接暴力的方式来做，两层循环。第一层循环假设拿到了第一个数，这样就根据target number可以得到第二个数，然后第二层循环就去找第二个数。找到了之后，给角标+1就可以返回了。   
但是这样太暴力了，所以第二层就采用二分查找的方式来找，可以节省一点时间。



## Solution
### My Solution
```java
public class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int[] result = new int[2];
        for (int i = 0; i < numbers.length - 1; i++) {
            int temp = target - numbers[i];
            int search = binarySearch(numbers, temp);
            if(search != -1 && search != i) {
                result[0] = i + 1;
                result[1] = search + 1;
                return result;
            }
        }
        return result;
    }

    public int binarySearch(int[] arry, int des) {
        int low = 1;
        int high = arry.length - 1;
        int middle;
        while(low <= high) {
            middle = (low + high) / 2;
            if(des == arry[middle]) {
                return middle;
            } else if(des < arry[middle]) {
                high = middle - 1;
            } else if(des > arry[middle]) {
                low = middle + 1;
            }
        }
        return -1;
    }
}
```
