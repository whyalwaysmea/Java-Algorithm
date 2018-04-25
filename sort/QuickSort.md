# 快速排序  
思路：  
1. 在待排序的元素中任取一个元素作为基准（通常选最后一个），称为基准元素    
2. 将待排序的元素进行分区，比基准元素大的元素放在它的右边，比基准元素小的放在它的左边  
3. 对左右两个分区重复以上操作直到所有元素都是有序的   

示例： [5,8,3,6,4]  
1. 取4为基准，然后进行分区。[3,4,6,8,5]  
2. 对4两边的分区[3],[6,8,5]再分别进行分区，得到[3,4,5,8,6]  
3. 再进行分区排序得到[3,4,5,6,8]  

快排的核心思想我感觉就是选一个pivot（基准），然后将待排序数列通过与pivot比较分为两个部分A和B（A部分的数均小于或等于pivot，B部分均大于或等于pivot），这样划分好的数列中的数就处于A <= pivot <= B的状态，接着对划分好的两个部分A和B分别递归调用快排函数，最后一定能出现划分好的部分中只剩下一个元素或没有元素的情况，而对于只有一个元素的数列或空列，我们认为它已经是有序的，这样就把数列排好了。

所以在快排中怎样对数组进行划分是关键，不同版本的快速排序大体结构都差不多，不同之处就在于划分函数的不同，这里就暂时称它为Partition函数吧


## 经典快速排序    
```java
public class QuickSort {

    public void quickSort(int[] arr) {
        if (arr == null || arr.length < 2) {
            return;
        }
        quickSort(arr, 0, arr.length - 1);
    }

    public void quickSort(int[] arr, int l, int r) {
        if (l < r) {
            int[] p = partition(arr, l, r);
            quickSort(arr, l, p[0] - 1);
            quickSort(arr, p[1] + 1, r);
        }
    }
    
    public int[] partition(int arr[], int l, int r) {
        int left = l - 1;
        int right = r + 1;
        int num = arr[r];
        while(l < r) {
            if(arr[l] < num) {
                swap(arr, ++left, l++);
            } else if(arr[l] > num) {
                swap(arr, right--, l);
            } else {
                l++;
            }
        }
        return new int[]{left + 1, right-1};
    }
    
    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp; 
    }
}
```
这种方法是每次取分区的最后一个元素作为基准，这样会有一个问题。  
如果初始数据比较特殊，每次划分区的时候，只有左边区域或者右边区域，这样相当于每次只排好了一个数。  
比如初始数据：[1,2,3,4,5,6,7]，每次用最后一个数进行分区，那么所有时间复杂度依然是O(N^2)  


## 随机快速排序  
```java
public class QuickSort {
    public static void quickSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
		quickSort(arr, 0, arr.length - 1);
	}

	public static void quickSort(int[] arr, int l, int r) {
		if (l < r) {
            // 随机一下
			swap(arr, l + (int) (Math.random() * (r - l + 1)), r);
			int[] p = partition(arr, l, r);
			quickSort(arr, l, p[0] - 1);
			quickSort(arr, p[1] + 1, r);
		}
	}

	public static int[] partition(int[] arr, int l, int r) {
		int less = l - 1;
		int more = r;
		while (l < more) {
			if (arr[l] < arr[r]) {
				swap(arr, ++less, l++);
			} else if (arr[l] > arr[r]) {
				swap(arr, --more, l);
			} else {
				l++;
			}
		}
		swap(arr, more, r);
		return new int[] { less + 1, more };
	}

	public static void swap(int[] arr, int i, int j) {
		int tmp = arr[i];
		arr[i] = arr[j];
		arr[j] = tmp;
	}
}
``` 
随机快排，就是在选择基准数的时候，先进行一次随机交换，这样就可以保证每次分区后是相对平衡的。  

时间复杂度O(N*logN)，额外空间复杂度O(logN) 



# 参考  
[QuickSort快速排序的多种实现和优化](https://www.cnblogs.com/YuNanlong/p/6115953.html?utm_source=tuicool&utm_medium=referral)    
[快排，随机快排，双路快排，三路快排的理解](https://www.cnblogs.com/lyz1991/p/6329475.html)  
[【算法杂谈 1】 从一道面试题再看三路快排partition](https://www.imooc.com/article/16141)  