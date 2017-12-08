## [Description](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)
Given a sorted array, remove the duplicates in-place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

**Example:**
>Given nums = [1,1,2],  
Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
It doesn't matter what you leave beyond the new length.

给到一个排序好的数组，移除重复出现的元素，使得每个元素只能出现一次，并返回新的数组长度  

不要为了另外的一个数组分配额外的空间，

## [Discuss]()
**题意：**   
其实就是移除数组中重复的元素，但是不能用一个中间数据来进行处理，最多使用几个变量  

但是需要注意的是：这里需要保留重复元素中第一个出现的那个

**思考：**  
因为不能开辟额外的数组空间，所以只能对本来的数组进行自身的改变   
数组不像List，可以直接进行remove  
通常我们进行数组元素的改变就是赋值 
所以我们通过判断前后元素的值是否相等，如果相等，我们记录好第一次重复出现的位置（也就是相同元素中的第二个）,当出现了不相同的元素时，就将这个不同的元素移动到刚才记录的那个位置去  

这里有点双指针的概念，一个指针去遍历，一个指针作为标志位

**优化:**   


## Solution
**MySolution：**   
```java
public int removeDuplicates(int[] nums) {
    if(nums.length == 0) {
        return 0;
    }
    int i = 0;
    for(int j = 0; j < nums.length; j++) {
        if(nums[i] != nums[j]) {
            i++;
            nums[i] = nums[j];
        }
    }
    return i + 1;
}
```


**Better Solution：**  
