# 在行列都排好序的矩阵中找数
【题目】给定一个有N*M的整形矩阵matrix和一个整数k，matrix的每一行和每一列都是排好序的。实现一个函数，判断k是否在matrix中。    

【要求】时间复杂度为O(M+N)，额外空间复杂度为O(1)  

【示例】给定的matrix为： 
>0, 1, 2, 5,   
2, 3, 4, 7,  
4, 4, 9, 18,     
5, 7, 10, 19 

如果k为9，返回true；如果k为6，返回false

# 思考 
因为行和列是排好序的，所以我们以右上角或者左下角的点为起点，然后进行比较。  

以上面的matrix为例，k为9。我们选择右上角`matrix[0][3]`为起始比较点： 
1. matrix[0][3] < 9，因为matrix[0][3]已经是第0行最大的值，所以往下移动后再比较  
2. matrix[1][3] < 9，继续下移  
3. matrix[2][3] > 9，所以不必再下移，而是往左移动 
4. matrix[2][2] = 9，返回true  





# 算法实现 
```java
public class FindNumInSortedMatrix {

    public static boolean isContains(int[][] matrix, int K) {
        int row = 0;
        int col = matrix[0].length - 1;
        while (row < matrix.length && col > -1) {
            if (matrix[row][col] == K) {
                return true;
            } else if (matrix[row][col] > K) {
                col--;
            } else {
                row++;
            }
        }
        return false;
    }
}

```