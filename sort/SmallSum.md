# 小和问题  
在一个数组中，每一个数左边比当前数小的数累加起来，叫做这个数组的小和。 

例子：[1,3,4,2,5]  
1左边比1小的数，没有  
3左边比3小的数，1   
4左边比4小的数，1，3     
2左边比2小的数，1  
5左边比5小的数，1，3，4，2  
所以小和为 1+ 1+3 + 1 + 1+3+4+2 = 16

# 算法实现  
## 遍历  
比较容易想到的就是遍历实现了，双层循环
```java
public int smallSum(int arr[]) {
    int result;
    for(int i = 0; i < arr.length; i++) {
        for(int j = 0; j < i; j++) {
            if(arr[i] > arr[j]) {
                result += arr[j];
            }
        }
    }
    return result;
}
```

时间复杂度：O(N^2) 

## 归并   
这个问题其实可以换一个角度来看待，每个数的右边有多少个大于当前数的数字，然后个数*当前数， 累加起来就是了。
我们可以回忆一下[归并算法](https://github.com/whyalwaysmea/Java-Algorithm/blob/master/sort/MergeSort.md)，将左右两个有序序列进行合并的时候会有一个排序的操作，此时就是进行统计的好时机。  
假设两个有序序列为arrA，arrB，指针所指位置分别为p1，p2。因为左右都是排序好的，所以当合并时，左边指针所指数小于右边指针所指数时，右边的序列中就有arrB.length - p2个数大于p1当前所指的数 
```java
public class SmallSum() {
    public static int smallSum(int[] arr) {
		if (arr == null || arr.length < 2) {
			return 0;
		}
		return mergeSort(arr, 0, arr.length - 1);
	}

	public static int mergeSort(int[] arr, int l, int r) {
		if (l == r) {
			return 0;
		}
		int mid = l + ((r - l) >> 1);
		return mergeSort(arr, l, mid) + mergeSort(arr, mid + 1, r) + merge(arr, l, mid, r);
	}

	public static int merge(int[] arr, int l, int m, int r) {
		int[] help = new int[r - l + 1];
		int i = 0;
		int p1 = l;
		int p2 = m + 1;
		int res = 0;
		while (p1 <= m && p2 <= r) {
            // 在此进行统计
			res += arr[p1] < arr[p2] ? (r - p2 + 1) * arr[p1] : 0;
			help[i++] = arr[p1] < arr[p2] ? arr[p1++] : arr[p2++];
		}
		while (p1 <= m) {
			help[i++] = arr[p1++];
		}
		while (p2 <= r) {
			help[i++] = arr[p2++];
		}
		for (i = 0; i < help.length; i++) {
			arr[l + i] = help[i];
		}
		return res;
	}
}
```