# 堆排序  
堆排序(Heapsort)是指利用堆积树（堆）这种数据结构所设计的一种排序算法  

在了解堆这种数据结构之前，我们需要了解一下完全二叉树 

## 满二叉树  
满二叉树是指除了最后一层外，每个节点都有两个孩子，而最后一层都是叶子节点，都没有孩子。 

![满二叉树](https://img-blog.csdn.net/20180412141921572?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0MzU4OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 完全二叉树  
满二叉树一定是完全二叉树，但完全二叉树不要求最后一层是满的，但如果不满，则要求所有节点必须集中在最左边，从左到右是连续的，中间不能有空的。

![完全二叉树](https://img-blog.csdn.net/20180412142104462?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0MzU4OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

完全二叉树有一个重要的特点：给定任意一个节点，可以根据其编号直接快速计算出其父节点和孩子节点。如果编号为i，则父节点编号为(i-1)/2，左孩子编号为2*i+1，右孩子编号为2*i+2 

![完全二叉树特点](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217192038651-934327647.png)


## 堆
堆逻辑概念上是一棵完全二叉树，而物理存储上使用数组，还要一定的顺序要求。  
在堆中，可以有重复元素，元素间不是完全有序的，但对于父子节点直接，有一定的顺序要求。根据顺序分为两种堆：最大堆、最小堆  


## 最大堆/最小堆
最大堆是指每个节点都不大于其父节点。这样，对于每个父节点，一定不小于其所有孩子节点，那么根节点就是所有节点中最大的了。最小堆与最大堆就正好相反，每个节点都不小于其父节点。  
最大堆中的每一个子树，也是一个最大堆 

![最大堆/最小堆](https://img-blog.csdn.net/20180412145713342?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM0MzU4OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
 

# 基本思路   
堆排序的基本思想是：将待排序序列构造成一个最大堆，此时，整个序列的最大值就是堆顶的根节点。将其与末尾元素进行交换，此时末尾就为最大值。然后将剩余n-1个元素重新构造成一个堆，这样会得到n个元素的次小值。如此反复执行，便能得到一个有序序列了  

## 构建最大堆  
因为最大堆的子树也是一个最大堆，所以我们可以从头开始进行最大堆的构建。   

假定数组为：[4,6,8,5,9]
1. 假设第一个元素[4]自己构建成最大堆，然后将第二个元素[6]插入，现在为[4,6]，  
2. 此时4为父节点，6为子节点，6大于4，所以交换它们俩。变成[6,4]。然后将三个元素[8]加入，现在为[6,4,8] 
3. 此时6为父节点，4是左孩子，8是右孩子。8>6，所以交换它俩的位置。变成[8,4,6]。 然后将四个元素[5]加入，现在为[8,4,6,5] 
4. 此时5是4的子节点，5>4，所以交换它俩，变成[8,5,6,4]。 然后将第五个元素[9]加入 
5. 此时9是5的子节点，9>5，交换它俩位置，变成[8,9,6,4,5]。此时9是8的子节点，9>8，交换位置，变成[9,8,6,4,5]

所以构建最大堆的思路：一个一个的插入到最大堆中，插入的之后和父节点进行比较，然后进行相应的调整，直到成为最大堆

```java
public static void heapSort(int[] arr) {
    if (arr == null || arr.length < 2) {
        return;
    }
    // 构建最大堆
    for (int i = 0; i < arr.length; i++) {
        heapInsert(arr, i);
    }
    
}

/**
 * @param index 当前加入的元素下标
 */
public static void heapInsert(int[] arr, int index) {
    // 如果当前元素大于父节点，则交换
    while (arr[index] > arr[(index - 1) / 2]) {
        swap(arr, index, (index - 1) / 2);
        index = (index - 1) / 2;
    }
}

public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}
```
## 堆的调整  
一个已经构建好的最大堆，如果我们修改其中一个元素，将该元素调小，那么该如何操作又将其重新排列成一个最大堆呢？  
因为是将一个元素调小了，所以只需要将该元素和它的子节点进行比较，再调整就可以了。比较的时候，应该是和左右子节点中较大的那个进行比较。
```java
/**
 * @param arr       需要调整的数组
 * @param index     调整元素的下标
 * @param size      数组的长度
 */
public static void heapify(int[] arr, int index, int size) {
    // 左孩子
    int left = index * 2 + 1;
    // 确保没有越界
    while (left < size) {
        // 获取左右子节点中较大的那个
        int largest = left + 1 < size && arr[left + 1] > arr[left] ? left + 1 : left;
        // 获取调整后的父元素和较大子节点中的那个较大值
        largest = arr[largest] > arr[index] ? largest : index;
        // 如果调整后的父节点依然是较大的，就不用做调整了
        if (largest == index) {
            break;
        }
        // 如果子节点更大，那么交换位置
        swap(arr, largest, index);
        // 下标交换，进行下一次比较
        index = largest;
        left = index * 2 + 1;
    }
}
```

## 排序  
在构建好最大堆之后，我们就可以根据堆的结构特点进行排序了。 
因为是构建的最大堆，所以根节点总是最大的，那么我们可以将根节点和最后一个叶节点交换，此时最后一个节点就是最大值了，然后再将除最后一个元素的堆进行调整，重新调整成一个最大堆，然后重复这个步骤。  

示例： 最大堆构建后的数组为[9, 8, 6, 4, 5] 
1. 将9和5进行交换，[5,8,6,4,9]此时最后一个元素就是最大值了  
2. 将[5,8,6,4]进行重新构建，构建成一个最大堆[8, 5, 6, 4]。 此时将8和4进行交换，然后将8排除开，再次进行构建。 
3. 重复上面的步骤

```java
public static void heapSort(int[] arr) {
    if (arr == null || arr.length < 2) {
        return;
    }
    for (int i = 0; i < arr.length; i++) {
        heapInsert(arr, i);
    }
    int size = arr.length;
    swap(arr, 0, --size);
    while (size > 0) {
        heapify(arr, 0, size);
        swap(arr, 0, --size);
    }
}

public static void heapInsert(int[] arr, int index) {
    while (arr[index] > arr[(index - 1) / 2]) {
        swap(arr, index, (index - 1) / 2);
        index = (index - 1) / 2;
    }
}

public static void heapify(int[] arr, int index, int size) {
    int left = index * 2 + 1;
    while (left < size) {
        int largest = left + 1 < size && arr[left + 1] > arr[left] ? left + 1 : left;
        largest = arr[largest] > arr[index] ? largest : index;
        if (largest == index) {
            break;
        }
        swap(arr, largest, index);
        index = largest;
        left = index * 2 + 1;
    }
}
```

# 复杂度 稳定性  
时间复杂度：O(N*logN)。   
空间复杂度：O(1) 

不稳定
