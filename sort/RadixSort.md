# 基数排序  
基数排序(Radix Sort)是桶排序的扩展，它的基本思想是：将整数按位数切割成不同的数字，然后按每个位数分别比较。
具体做法是：将所有待比较数值统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后, 数列就变成一个有序序列。 

示例数组：{53, 3, 542, 748, 14, 214, 154, 63, 616}  

1. 对原始数据按个位排序：{542, 053, 003, 063, 014, 214, 154, 616, 748}  
2. 对经过1过程的数组按十位排序：{003, 014, 214, 616, 542, 748, 053, 154, 616} 
3. 对经过2过程的数组按百位排序：{003, 014, 053, 063, 154, 214, 542, 616, 748}


# 算法实现  
```java
public static void radixSort(int[] arr) {
    if (arr == null || arr.length < 2) {
        return;
    }
    radixSort(arr, 0, arr.length - 1, maxbits(arr));
}

// 获取最大数有多少位
public static int maxbits(int[] arr) {
    int max = Integer.MIN_VALUE;
    for (int i = 0; i < arr.length; i++) {
        max = Math.max(max, arr[i]);
    }
    int res = 0;
    while (max != 0) {
        res++;
        max /= 10;
    }
    return res;
}

/**
 * 进行排序 
 * @param arr       待排序数组
 * @param begin     数组开始的位置
 * @param end       数组结束的位置
 * @param digit     数组中最大数的位数
 */
public static void radixSort(int[] arr, int begin, int end, int digit) {
    final int radix = 10;
    int i = 0, j = 0;
    int[] count = new int[radix];
    // 临时排序用的桶
    int[] bucket = new int[end - begin + 1];
    // d表示当前是哪一位， 1是个位 
    for (int d = 1; d <= digit; d++) {
        // 复位
        for (i = 0; i < radix; i++) {
            count[i] = 0;
        }
        for (i = begin; i <= end; i++) {
            // arr[i]当前位上的数字
            j = getDigit(arr[i], d);
            count[j]++;
        }
        // 计数排序
        for (i = 1; i < radix; i++) {
            count[i] = count[i] + count[i - 1];
        }
        for (i = end; i >= begin; i--) {
            j = getDigit(arr[i], d);
            bucket[count[j] - 1] = arr[i];
            count[j]--;
        }
        // 将排序后的数组放回arr
        for (i = begin, j = 0; i <= end; i++, j++) {
            arr[i] = bucket[j];
        }
    }
}

/**
 * 获取数 指定位的数字
 * @param x 数字
 * @param d 指定的位
 */
public static int getDigit(int x, int d) {
    return ((x / ((int) Math.pow(10, d - 1))) % 10);
}

```


# 效率分析
时间复杂度为O(d(n+radix))  
d表示最大数有多少位  
radix表示单位的数的取值范围  
n表示元素个数 



稳定算法