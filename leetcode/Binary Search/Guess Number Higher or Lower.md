## [Description](https://leetcode.com/problems/guess-number-higher-or-lower/description/)
We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number is higher or lower.

You call a pre-defined API guess(int num) which returns 3 possible results (-1, 1, or 0):

>-1 : My number is lower
 1 : My number is higher
 0 : Congrats! You got it!

---
>n = 10, I pick 6.
Return 6.


## [Discuss]()
**题意：**   
其实就是猜数字，如果你猜大了就返回1，猜小了就返回-1，正确就返回0

**思考：**  
最简单的就是遍历了，挨着猜

**优化:**   
使用二分查找了，给定了一个范围，从中间开始猜。

二分查找的核心思想就是：通过中位数来判断，进而缩短查找的范围 
其实我们可以用三元搜索的方式： 
我们将排好序的数组平分成三分，选择1/3的点为m1，2/3的点为m2。
如果结果数字小于m1，那么就继续在最小数和m1之间查找，如果结果数字大于m2，那么在m2和最大数之间查找，如果在大于m1小于m2，那么就继续在m1和m2之间查找

## Solution
**MySolution：**   
```java
/* The guess API is defined in the parent class GuessGame.
   @param num, your guess
   @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
      int guess(int num); */

public class Solution extends GuessGame {
    public int guessNumber(int n) {
        for (int i = 1; i < n; i++)
            if (guess(i) == 0)
                return i;
        return n;
    }
}
```
时间复杂度：O(n)
空间复杂度：O(1)

**Better Solution：**  
```java
public class Solution extends GuessGame {
    public int guessNumber(int n) {
		int low = 1;
        int high = n;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            int res = guess(mid);
            if (res == 0)
                return mid;
            else if (res < 0)
                high = mid - 1;
            else
                low = mid + 1;
        }
        return -1;
    }
}
```
时间复杂度：O($log_2 x$)
空间复杂度：O(1)


## Better 
```java
public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int low = 1;
        int high = n;
        while (low <= high) {
            int mid1 = low + (high - low) / 3;
            int mid2 = high - (high - low) / 3;
            int res1 = guess(mid1);
            int res2 = guess(mid2);
            if (res1 == 0)
                return mid1;
            if (res2 == 0)
                return mid2;
            else if (res1 < 0)
                high = mid1 - 1;
            else if (res2 > 0)
                low = mid2 + 1;
            else {
                low = mid1 + 1;
                high = mid2 - 1;
            }
        }
        return -1;
    }
}
```
时间复杂度：O($log_3 x$)
空间复杂度：O(1)
