# [Description](https://leetcode.com/problems/invert-binary-tree/description/)
Invert a binary tree.
![这里写图片描述](http://img.blog.csdn.net/20180123164737983?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzQzNTg5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

to

![这里写图片描述](http://img.blog.csdn.net/20180123164748413?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzQzNTg5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


## [Discuss]()
**题意：**   
题意还是比较清楚的，就是翻转二叉树 

**思考：**  
直接使用递归，递归到后面的时候进行左右的交换  


**另一种思路:**   
可以采用广度优先遍历（Breadth First Search）  
广度优先遍历算法，又叫宽度优先遍历，或横向优先遍历，是从根节点开始，沿着树的宽度遍历树的节点。如果所有节点均被访问，则算法中止。  

![BFS](http://img.blog.csdn.net/20150804211141419) 
如上图所示的二叉树，A 是第一个访问的，然后顺序是 B、C，然后再是 D、E、F、G。 
那么，怎样才能来保证这个访问的顺序呢？ 
借助队列数据结构，由于队列是先进先出的顺序，因此可以先将左子树入队，然后再将右子树入队。 
这样一来，左子树结点就存在队头，可以先被访问到。

在了解了BFS之后，应该思路就很清晰了。因为它是先将左子树放入，然后再放入右子树。 那么我们就可以在这个时候进行位置的交换  
 
## Solution
**MySolution：**   
```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) {
        return null;
    }
    TreeNode right = invertTree(root.right);
    TreeNode left = invertTree(root.left);
    root.left = right;
    root.right = left;
    return root;
}
```

**Another Solution：**  
```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) return null;
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.add(root);
    while (!queue.isEmpty()) {
        TreeNode current = queue.poll();
        TreeNode temp = current.left;
        current.left = current.right;
        current.right = temp;
        if (current.left != null) queue.add(current.left);
        if (current.right != null) queue.add(current.right);
    }
    return root;
}
```