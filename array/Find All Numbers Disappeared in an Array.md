## [Description](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/#/description)
Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements of [1, n] inclusive that do not appear in this array.

Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

**Example:**
```java
Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]
```

## [Discuss](https://discuss.leetcode.com/category/575/find-all-numbers-disappeared-in-an-array)
**题意：**   
给出一个整数数组，这个数组满足1 ≤ a[i] ≤ n (n 是数组的大小),也就是说每个元素是大于等于1，小于等于n的。   
有的元素可能出现了两次，有的出现了一次。   
找出[1,n]中没有在数组中出现过的元素。

**思考：**  
我们可以通过遍历数组，将出现的元素放入一个HashMap中，key和value都是元素的值。   
然后进行第二次循环[0,n],然后去查看HashMap中是否有相对于的key，如果没有，那么i值在数组中也是没有的。

**优化:**   
因为数组中的元素都在[1,n]的范围内，所以可以取出元素然后减一，将这个值作为下标，再去取数组的元素:  nums[nums[i] - 1]。   
此时我们就可以改变一下取得的元素值+n。nums[nums[i] - 1]+n   
因为有的元素出现了两次，所以有可能刚才改变的元素又被取到了，所以在第一步取下标的时候就应该做下处理:  nums[(nums[i] - 1) % n]。
最后再遍历一次数组，如果取得的元素值是小于n的，那么该下标+1的值就没有出现过。  

当然这里也可以将取出来的值变为负数， 第二次取下标的时候取绝对值也是一样的。

## Solution
**MySolution：**
```java
public class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> result = new ArrayList<>();
        HashMap<Integer, Integer> temp = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if(!temp.containsKey(Integer.valueOf(nums[i]))) {
                temp.put(Integer.valueOf(nums[i]), Integer.valueOf(nums[i]));
            }
        }
        for (int i = 1; i <= nums.length; i++) {
            if(!temp.containsKey(Integer.valueOf(i))) {
                result.add(i);
            }
        }
        return result;
    }
}
```

**Better Solution：** 
```java
public List<Integer> findDisappearedNumbers(int[] nums) {
    List<Integer> res = new ArrayList<>();
    int n = nums.length;
    for (int i = 0; i < nums.length; i ++) nums[(nums[i]-1) % n] += n;
    for (int i = 0; i < nums.length; i ++) if (nums[i] <= n) res.add(i+1);
    return res;
}
```
