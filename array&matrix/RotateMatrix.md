# 旋转正方形矩阵 
【题目】 给定一个整形正方形矩阵matrix，请把该矩阵调整成顺时针旋转90°的样子 
【要求】 额外空间复杂度为O(1)

【示例】   
旋转前：
>1,2,3,4,  
5,6,7,8,  
9,10,11,12,  
13,14,15,16,  

旋转后：
>13,9,5,1,  
14,10,6,2,  
15,11,7,3,   
16,12,8,4,  


# 思考 
先考虑一圈的旋转，其实上是边的旋转，再拆分可以成为点的交换。  


# 实现  
```java
public class RotateMatrix {

    public static void rotate(int[][] matrix) {
        int tR = 0;
        int tC = 0;
        int dR = matrix.length - 1;
        int dC = matrix[0].length - 1;
        while(tR < dR) {
            rotateEdge(matrix, tR++, tC++, dR--, dC--);
        }
    }
	
    public static void rotateEdge(int[][] m, int tR, int tC, int dR, int dC) {
        int times = dC - tC;
        int tmp = 0;
        for(int i = 0; i < times; i++) {
            tmp = m[tR][tC + i];                // A -> tmp
            m[tR][tC + i] = m[dR - i][tC];      // D -> A
            m[dR - i][tC] = m[dR][dC - i];      // C -> D 
            m[dR][dC - i] = m[tR + i][dC];      // B -> C 
            m[tR + i][dC] = tmp;                // tmp -> A
        }
    }
}
```

