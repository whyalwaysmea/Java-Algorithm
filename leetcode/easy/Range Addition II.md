## [Description](https://leetcode.com/problems/range-addition-ii/#/description)
Given an m * n matrix M initialized with all 0's and several update operations.

Operations are represented by a 2D array, and each operation is represented by an array with two positive integers a and b, which means M[i][j] should be added by one for all 0 <= i < a and 0 <= j < b.

You need to count and return the number of maximum integers in the matrix after performing all the operations.

**Example 1:**  
```java
Input:
m = 3, n = 3
operations = [[2,2],[3,3]]
Output: 4
Explanation:
Initially, M =
[[0, 0, 0],
 [0, 0, 0],
 [0, 0, 0]]

After performing [2,2], M =
[[1, 1, 0],
 [1, 1, 0],
 [0, 0, 0]]

After performing [3,3], M =
[[2, 2, 1],
 [2, 2, 1],
 [1, 1, 1]]

So the maximum integer in M is 2, and there are four of it in M. So return 4.
```

**Note:**
1. The range of m and n is [1,40000].
2. The range of a is [1,m], and the range of b is [1,n].
3. The range of operations size won't exceed 10,000.

## [Discuss](https://discuss.leetcode.com/category/760/range-addition-ii)
**题意：**   
给定一个M * N 的矩阵，初始化的时候里面的元素全都为0   
然后给出一系列的更新操作   
更新操作是一个二维数组，每个操作用一个带有两个正整数a和b的数组表示，对于所有满足0 <= i < a and 0 <= j < b.的M[i][j]应该加一   
最后找出矩阵中最大元素的出现次数

**思考：**  
最开始想的是直接把矩阵初始化出来，然后通过遍历ops，先判断a，b的范围然后进行范围内元素的更新。  
然后遍历矩阵，用一个HashMap来保存每一个元素，以元素为key，出现次数为value，同时记录最大的那个元素。  
最后根据记录的最大元素，从HashMap中得到元素出现的次数。   

**优化:**   
如果直接初始化矩阵的话，当m和n很大的时候(40000)会出现OOM，所以最开始的想法虽然思路是正确的，但是在实际的调试过程中是会出现问题的。   
根据刚才的思路，其实是否初始化矩阵并不关键，关键在于遍历ops，然后判断a，b的范围去进行了更新操作。      
所以我们只需要找出那个范围内出现次数最多的那个小矩阵就可以了，也就是最小的a和最小的b。当然a和b要分别小于m，n。  
最后minA * minB 就是最大元素的出现次数了。


## Solution
**MySolution：**   
```java
public class Solution {
    public int maxCount(int m, int n, int[][] ops) {
        int minM = Integer.MAX_VALUE;
        int minN = Integer.MAX_VALUE;
        for (int i = 0; i < ops.length; i++) {
            int[] op = ops[i];
            minM = Math.min(minM, op[0]);
            minN = Math.min(minN, op[1]);
        }
        minM = Math.min(m, minM);
        minN = Math.min(n, minN);

        return minM * minN;
    }
}
```

**Better Solution：**  
```java
public class Solution {
    public int maxCount(int m, int n, int[][] ops) {
        int rowMin = m;
        int colMin = n;
        for (int[] pair : ops) {
            rowMin = Math.min(rowMin, pair[0]);
            colMin = Math.min(colMin, pair[1]);
        }
        return rowMin * colMin;
    }
}
```
