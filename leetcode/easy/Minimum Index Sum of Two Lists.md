## [Description](https://leetcode.com/problems/minimum-index-sum-of-two-lists/#/description)
Suppose Andy and Doris want to choose a restaurant for dinner, and they both have a list of favorite restaurants represented by strings.

You need to help them find out their common interest with the least list index sum. If there is a choice tie between answers, output all of them with no order requirement. You could assume there always exists an answer.

**Example 1:**  
```java
Input:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]
Output: ["Shogun"]
Explanation: The only restaurant they both like is "Shogun".
```

**Example 2:**     
```java
Input:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["KFC", "Shogun", "Burger King"]
Output: ["Shogun"]
Explanation: The restaurant they both like and have the least index sum is "Shogun" with index sum 1 (0+1).
```

**Note:**   
1. The length of both lists will be in the range of [1, 1000].
2. The length of strings in both lists will be in the range of [1, 30].
3. The index is starting from 0 to the list length minus 1.
4. No duplicates in both lists.

## [Discuss](https://discuss.leetcode.com/category/761/minimum-index-sum-of-two-lists)
**题意：**   
有两个数组，找出里面相同的元素，如果这两个相同元素的下标和是最小的，则将它们输出

**思考：**  
双重遍历，先找到是否有相同的元素，然后获取它们的下标和，和原有的下标和进行比较。  
如果小于原来的和，那么放入到一个list中。

**另一个思路:**   
先用一个HashMap来装入list1，以元素为key，下标为value。  
遍历list2，取出元素，如果该元素在map1中有value，那么就将该元素存入map2中，以元素为key，sum为value，同时算出minSum  
最后遍历map2，如果value等于minSum那么就加入list中


## Solution
**MySolution：**   
```java
public class Solution {
    public String[] findRestaurant(String[] list1, String[] list2) {
        List<String> result = new ArrayList<>();
        int sum = Integer.MAX_VALUE;
        for (int i = 0; i < list1.length; i++) {
            String temp = list1[i];
            for (int j = 0; j < list2.length; j++) {
                if(temp.equals(list2[j])) {
                    if((i + j) == sum) {
                        result.add(temp);
                        sum = i + j;
                    } else if((i + j) < sum) {
                        result.clear();
                        result.add(temp);
                        sum = i + j;
                    }
                }
            }
        }
        String[] strings = new String[result.size()];
        return result.toArray(strings);
    }
}
```

**Another Solution：**  
```java
public class Solution {
  public String[] findRestaurant(String[] list1, String[] list2) {

        Map<String, Integer> map = new HashMap<>();
        Map<String, Integer> fi = new HashMap<>();
        List<String> res = new ArrayList<>();
        int min = Integer.MAX_VALUE;

        for (int i = 0; i < list1.length; i++)
            map.put(list1[i], i);

        for (int i = 0; i < list2.length; i++) {
            if (map.containsKey(list2[i])) {
                int indexsum = map.get(list2[i]) + i;
                fi.put(list2[i], indexsum);
                min = Math.min(min, indexsum);
            }
        }

        for (Map.Entry<String, Integer> entrySet : fi.entrySet())
            if (entrySet.getValue() == min)
                res.add(entrySet.getKey());

        return res.toArray(new String[res.size()]);
    }
}
```
