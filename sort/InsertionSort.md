# 插入排序  
插入排序的工作方式像排序一手扑克牌。 假设左手的牌是排序好的，桌面上的是未知的牌   
1. 开始时，我们的左手为空并且桌子上的牌面向下。    
2. 然后，我们每次从桌子上拿走一张牌并将它插入左手正确的位置。 为了找到插入的正确位置，我们将要插入的牌与左手的牌挨着比较，直接找到合适的位置并插入进去。 

在实际的实现过程中，我们可以将数组的第0个元素看成是已经排序好的，然后从第二个元素开始进行插入。  

# 算法实现 
```java
public class InsertionSort {

	public void insertionSort(int[] arr) {
		if(arr == null || arr.length < 2) {
			return ;
		}
		for (int i = 1; i < arr.length; i++) {
			for (int j = i; j > 0 && arr[j] < arr[j-1] ; j--) {
				swap(arr, j, j-1);
			}
		}
	}
	
	private void swap(int[] arr, int i, int j) {
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp; 
	}
}
```

# 时间复杂度  
因为每次插入的时候都需要去比较，所以时间复杂度为`O(N^2)` 

# 稳定性 
因为插入的数据是挨着顺序进入的，所以排序前后的相对顺序是不会改变的。所以是稳定的算法。  