# 转圈打印矩阵  
给定一个整形矩阵matrix，请按照转圈的方式打印它。 
【要求：】额外空间复杂度为O(1) 

【示例：】 
给定的矩阵：
>1,2,3,4   
5,6,7,8   
9,10,11,12   

打印结果：1，2，3，4，8，12，11，10，9，5，6，7

# 思考  
假设矩阵有row行，column列。那么矩阵的四个角A(0，0), B(0，column-1), C(row-1, 0), D(row-1, column-1)。   
我们可以看作是A-B，B-C，C-D，D-A这样就打印了一圈。    
然后将点往里层缩一圈再继续打印， 



# 实现
```java
public class PrintMatrixSpiralOrder {
	public static void spiralOrderPrint(int[][] matrix) {
		int tR = 0;
		int tC = 0;
		int dR = matrix.length - 1;
		int dC = matrix[0].length - 1;
		while (tR <= dR && tC <= dC) {
			printEdge(matrix, tR++, tC++, dR--, dC--);
		}
	}
	
	public static void printEdge(int[][] m, int tR, int tC, int dR, int dC) {
		if (tR == dR) {
            // 最后是一条横线
			for (int i = tC; i <= dC; i++) {
				System.out.print(m[tR][i] + " ");
			}
		} else if (tC == dC) {
            // 最后是一条竖线
			for (int i = tR; i <= dR; i++) {
				System.out.print(m[i][tC] + " ");
			}
		} else {
			int curC = tC;
			int curR = tR;
			// A-B
			while (curC != dC) {
				System.out.print(m[tR][curC] + " ");
				curC++;
			}
			// B-D
			while (curR != dR) {
				System.out.print(m[curR][dC] + " ");
				curR++;
			}
			// D-C
			while (curC != tC) {
				System.out.print(m[dR][curC] + " ");
				curC--;
			}
			// C-A
			while (curR != tR) {
				System.out.print(m[curR][tC] + " ");
				curR--;
			}
		}
	}

	public static void main(String[] args) {
		int[][] matrix = { { 1, 2, 3, 4 }, { 5, 6, 7, 8 }, { 9, 10, 11, 12 },
				{ 13, 14, 15, 16 } };
		spiralOrderPrint(matrix);

	}
}
```