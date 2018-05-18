# 之字形打印矩阵 
【示例】 
给定的数组
>1, 2, 3,   
4, 5, 6,  
7, 8, 9,   
10, 11, 12  

打印结果：1, 4, 2, 3, 5, 7, 10, 8, 6, 9, 11, 12

# 思考  
所谓的之字形打印，其实就是斜着打印直线，不过方向会变换的而已。  
我们只用先考虑怎么打印斜线就好了，因为是矩形，所以可能行先到尾或者列先到尾，那么主要问题就是边界的考虑。


# 算法实现
```java
public class ZigZagPrintMatrix {

    public static void printMatrixZigZag(int[][] matrix) {
        int tR = 0;     
        int tC = 0;     
        int dR = 0;     
        int dC = 0;
        // 最终的行
        int endR = matrix.length - 1;
        // 最终的列
        int endC = matrix[0].length - 1;
        // 表示之字的顺序
        boolean fromUp = false;             
        while (tR != endR + 1) {
            printLevel(matrix, tR, tC, dR, dC, fromUp);
            tR = tC == endC ? tR + 1 : tR;
            tC = tC == endC ? tC : tC + 1;
            dC = dR == endR ? dC + 1 : dC;
            dR = dR == endR ? dR : dR + 1;
            fromUp = !fromUp;
        }
    }

    public static void printLevel(int[][] m, int tR, int tC, int dR, int dC,
            boolean f) {
        if (f) {
            while (tR != dR + 1) {
                System.out.print(m[tR++][tC--] + " ");
            }
        } else {
            while (dR != tR - 1) {
                System.out.print(m[dR--][dC++] + " ");
            }
        }
    }

}
```