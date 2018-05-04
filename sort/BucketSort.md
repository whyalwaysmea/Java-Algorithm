# 桶排序  
桶排序，顾名思义就是运用桶的思想来将数据放到相应的桶内。一开始我们就人为的把桶设置来有序，然后将数据放入到相应的桶中，最后再按照桶的顺序挨着取数据，取出来的数据也就是有序的了。

把数组 arr 划分为n个大小相同子区间（桶），每个子区间各自排序，最后合并

1. 找出待排序数组中的最大值max、最小值min
2. 我们使用 动态数组ArrayList 作为桶，桶里放的元素也用 ArrayList 存储。桶的数量为(max-min)/arr.length+1
3. 遍历数组 arr，计算每个元素 arr[i] 放的桶
4. 每个桶各自排序
5. 遍历桶数组，把排序好的元素放进输出数组


# 算法实现  
```java
public static void bucketSort(int[] arr){
    
    int max = Integer.MIN_VALUE;
    int min = Integer.MAX_VALUE;
    for(int i = 0; i < arr.length; i++){
        max = Math.max(max, arr[i]);
        min = Math.min(min, arr[i]);
    }
    
    //桶数
    int bucketNum = (max - min) / arr.length + 1;
    ArrayList<ArrayList<Integer>> bucketArr = new ArrayList<>(bucketNum);
    for(int i = 0; i < bucketNum; i++){
        bucketArr.add(new ArrayList<Integer>());
    }
    
    //将每个元素放入桶
    for(int i = 0; i < arr.length; i++){
        int num = (arr[i] - min) / (arr.length);
        bucketArr.get(num).add(arr[i]);
    }
    
    //对每个桶进行排序
    for(int i = 0; i < bucketArr.size(); i++){
        Collections.sort(bucketArr.get(i));
    }

    // 最后将桶中的数据取出来
    int index = 0;
    for (int i = 0; i < bucketArr.size(); i++) {
        List<Integer> list = bucketArr.get(i);
        if(list.size() > 0) {
            for (int j = 0; j < list.size(); j++) {
                arr[index] = list.get(j);
                index++;
            }
        }
    }
    
}
```

 
# 时间复杂度  
只要所有桶的大小的平方和与元素总数成线性关系，桶排序就能在线性时间能完成。  

稳定性：因为每个桶中的排序算法不固定，因此会取决于里面的排序算法是否稳定  
