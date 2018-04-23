# 荷兰国旗问题  
初始的问题描述：
现有红、白、蓝三个不同颜色的小球，乱序排列在一起，请重新排列这些小球，使得红白蓝三色的同颜色的球在一起。这个问题之所以叫荷兰国旗，是因为我们可以将红白蓝三色小球想象成条状物，有序排列后正好组成荷兰国旗。 

问题延伸： 
给定一个数组arr，和一个数num，请把小于num的数放在数组的左边，等于num的数放在数组的中间，大于num的数放在数组的右边。要求额外空间复杂度O(1)，时间复杂度O(N)   

这里我们就直接以问题延伸为题进行学习  

# 算法思考  
因为从结果来看，排序后的数组是分成了三部分的，即左边（小于num），中间（等于num），右边（大于num）。那么必然会有两个指针，它们是用来划分区域的。  
我们一开始就可以定义这两个指针，left指向数组开始的位置，right指向数组末尾，然后开始遍历数组进行比较，同时还需要一个表明当前位置的指针current。 
current所指元素大于num的，就将current所指元和right所指元素交换，并且right左移；小于num的就将current所指元素和left所指元素交换，left右移，同时**current也右移**  

示例：arr =  {2,3,5,4,7} ， 指定num为4
1. 初始化三个指针，left：0， right：4， current：0  
2. arr[current] < 4 ， 所以arr[left]和arr[current]交换，left++，current++。 当前数据：[2,3,5,4,7], left：1， right：4， current： 1 
3. arr[current] < 4 ， 所以arr[left]和arr[current]交换，left++，current++。 当前数据：[2,3,5,4,7], left：2， right：4， current： 2   
3. arr[current] > 4 ， 所以arr[right]和arr[current]交换，right--。 当前数据：[2,3,7,4,5], left：2， right：3， current：2  
4. arr[current] > 4 ， 所以arr[right]和arr[current]交换，right--。 当前数据：[2,3,4,7,5], left：2， right：2， current：2     
5. arr[current] = 4 ， 所以arr[current]不变， current++

此时current已经大于right，就不用再进行遍历了  


# 算法实现 
```java
public class NetherlandsSort {

    public void partition(int arr[], int num) {
        int left = 0;
        int right = arr.length - 1;
        int current = 0;
        while(current <= right) {
            if(arr[current] < num) {
                swap(arr, left, current);
                left++;
                current++;
            } else if(arr[current] > num) {
                swap(arr, current, right);
                right--;
            } else {
                current++;
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

