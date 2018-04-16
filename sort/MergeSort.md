# 归并排序  
归并排序（MERGE-SORT）是利用归并的思想实现的排序方法，该算法采用经典的分治（divide-and-conquer）策略（分治法将问题分(divide)成一些小的问题然后递归求解，而治(conquer)的阶段则将分的阶段得到的各答案"修补"在一起，即分而治之)。    

首先我们可以思考：如何将两个有序的序列合并，并且合并后依然有序呢？  
这个比较简单，我们只需要用两个指针，一开始都指向两个序列的头部，然后开始比较两个指针所指数的大小，将小的那个放入一个新建的空间中，然后将它从相应的队列中删除，同时相应队列的指针后移一位；如此往复这个操作，那么两个有序的序列就合并了。   
```java
public int[] merge(int arrA[], int arrB[]) {
    // 创建合并后的数组
    int[] result = new int[arrA.length + arrB.length];
    int p1 = 0; // arrA的指针
    int p2 = 0; // arrB的指针
    int i = 0;  // 当前要放入result的位置
    // 比较两个指针所指数的大小，将小的那个放入新的数组中，同时相应队列的指针后移
    while(p1 < arrA.length && p2 < arrB.length) {
        result[i++] = arrA[p1] < arrB[p2] ? arrA[p1++] : arrB[p2++];
    }

    // 现在必有一个序列已经过完了，下面的循环只会进入一个
    while(p1 < arrA.length) {
        // 当前arrB走完了，将arrA中剩下的直接放入结果数组中
        result[i++] = arrA[p1++];
    }

    while(p2 < arrB.length) {
        // 当前arrA走完了，将arrB中剩下的直接放入结果数组中
        result[i++] = arrB[p2++];
    }
    return result;
}
```

归并排序就用到了这样的思想：先把大的数组拆分成两个序列，将这两个序列分别进行排序，然后再合并起来。  
可能有人会想，拆分成的两个序列又怎么排序呢？很简单，我们继续将它拆分，当其不能拆分了，就认为这个不能拆分的个体已经排好序了，然后就可以进行合并了。  

假设初始的数组是`[5,6,4,7,3,2]`，我们可以先将其分成两个小的部分进行排序，进而再合并起来。  
1. 首先拆分成两个部分：`[5,6,4]`和`[7,3,2]`,将这两个部分分别排序后，再用外排的方式将两者合并排序起来。但是这两个部分怎么分别排序呢？我们可以继续拆分  
2. 将`[5,6,4]`和`[7,3,2]`两个部分再进行拆分，得到：`[5,6]`,`[4]`和`[7,3]`,`[2]`；现在又可以继续将它们拆分  
3. 继续拆分得到{`[5]`,`[6]`和`[4]`} {`[7]`和`[3]`,`[2]`}； 现在已经无法继续拆分了，就可以进行合并了
4. 第一次归并后：`[5,6]`和`[4]`，`[3,7]`和`[2]`
5. 第二次归并后：`[4,5,6]`和`[2,3,7]`  
6. 第三次归并后：`[2,3,4,5,6,7]`


# 算法实现 
```java
public class MergeSort {

	public void mergeSort(int[] arr) {
		if(arr == null || arr.length < 2) {
			return ;
		}
		mergeSort(arr, 0, arr.length - 1);
	}
	
	/**
	 * 将数组在指定范围内进行拆分合并
	 */
	public void mergeSort(int[] arr, int l, int r) {
		// 已经无法再拆分了
		if(l == r) {
			return ;
		}
		// 从中间拆分
		int mid = l + ((r-l) >> 1);
		mergeSort(arr, l, mid);
		mergeSort(arr, mid + 1, r);
		// 进行合并
		merge(arr, l, mid, r);
	}
	
	/**
	 * 合并，l到m，m+1到r 两段分别有序
	 */
	public void merge(int[] arr, int l, int m, int r) {
		int[] help = new int[r - l + 1];
		int i = 0;
		int p1 = l;
		int p2 = m + 1;
		while (p1 <= m && p2 <= r) {
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
	}
}

``` 


## 时间复杂度  
这里到了递归的算法，每一个递归都可以转换成非递归的方式。  
递归分治法有一个[Master公式](http://www.gocalf.com/blog/algorithm-complexity-and-master-theorem.html)，可以用来求解时间复杂度   
简单的提一下Master公式：
T(N) = a * T(N/b) + O(N^d)， N表示样本量，N/b表示子过程的样本量，a表示子过程执行次数，O(N^d)表示除去子过程之外，剩下的时间复杂度  

我们这里是将数组从中间进行拆分，分成两段分别排序，所以子过程样本量是N/2；左右两个子过程，所以执行两次，a为2；剩下了一个合并的排序，时间复杂度为O(N) 
所以最终的Master公式是：T(n)=2T(n/2)+O(N)，

得到公式后，就可以去套答案了： 
1. log(b,a) > d: 时间复杂度O(N^log(b,a))  
2. log(b,a) = d: 时间复杂度O(N^d * logN)  
3. log(b,a) < d: 时间复杂度O(N^d)  


所以最终的时间复杂度是：O(N*logN)  

## 空间复杂度 
O(N)