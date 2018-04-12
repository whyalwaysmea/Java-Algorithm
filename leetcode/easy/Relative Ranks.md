## [Description](https://leetcode.com/problems/relative-ranks/#/description)
Given scores of N athletes, find their relative ranks and the people with the top three highest scores, who will be awarded medals: "Gold Medal", "Silver Medal" and "Bronze Medal".

**Example 1:**   
```java
Input: [5, 4, 3, 2, 1]   
Output: ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]   
Explanation: The first three athletes got the top three highest scores, so they got "Gold Medal", "Silver Medal" and "Bronze Medal".
For the left two athletes, you just need to output their relative ranks according to their scores.
```

**Note:**
1. N is a positive integer and won't exceed 10,000.
2. All the scores of athletes are guaranteed to be unique.

## [Discuss](https://discuss.leetcode.com/category/655/relative-ranks)
**题意：**   
给出N个数，返回这些数的排名。第一名为"Gold Medal", 第二名为"Silver Medal", 第三名为"Bronze Medal"，其他的就从4开始挨着顺序输出数字。  

**思考：**  
先对原来的数据进行排序，然后遍历已经排序的数据，查找每个数在原来数组中的位置。  
这样就可以得到原来数组中的每一个数的名次了，只是需要在前三名的时候，将值修改一下就好了。



## Solution
### MySolution
```java
public class Solution {
    public String[] findRelativeRanks(int[] nums) {
        // 拷贝一份原有数组
        int[] temp = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            temp[i] = nums[i];
        }
        String[] result = new String[nums.length];
        // 对数组进行排序
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {       
            // 查找第i个数，是多少名(从小到大排序的)
            int search = Arrays.binarySearch(nums, temp[i]);
            if(search == nums.length - 1) {
                result[i] = "Gold Medal";
            } else if(search == nums.length - 2) {
                result[i] = "Silver Medal";
            } else if(search == nums.length - 3) {
                result[i] = "Bronze Medal";
            } else {
                result[i] = "" + (nums.length - search);
            }
        }

        return result;
    }
}
```


### Another Solution
```java
public class Solution {
    public String[] findRelativeRanks(int[] nums) {
        String[] result = new String[nums.length];
        // 找到数组中的最大数
        int max = 0;
        for (int i : nums) {
            if (i > max) max = i;
        }
        // 新建一个数组，以原来成绩为角标进行赋值
        int[] hash = new int[max + 1];
        for (int i = 0; i < nums.length; i++) {
            hash[nums[i]] = i + 1;
        }
        int place = 1;
        // 其实是间接的排序了
        for (int i = hash.length - 1; i >= 0; i--) {
            if (hash[i] != 0) {
                if (place == 1) {
                    result[hash[i] - 1] = "Gold Medal";
                } else if (place == 2) {
                    result[hash[i] - 1] = "Silver Medal";
                } else if (place == 3) {
                    result[hash[i] - 1] = "Bronze Medal";
                } else {
                    result[hash[i] - 1] = String.valueOf(place);
                }
                place++;
            }
        }
        return result;
    }
}
```
