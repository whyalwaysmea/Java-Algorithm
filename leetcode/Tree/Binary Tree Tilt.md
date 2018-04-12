## [Description](https://leetcode.com/problems/binary-tree-tilt/#/description)
Given a binary tree, return the tilt of the whole tree.

The tilt of a tree node is defined as the absolute difference between the sum of all left subtree node values and the sum of all right subtree node values. Null node has tilt 0.

The tilt of the whole tree is defined as the sum of all nodes' tilt.

**Example:**
![Example](http://upload-images.jianshu.io/upload_images/293077-2772cc3c58088198.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## [Discuss](https://discuss.leetcode.com/category/720/binary-tree-tilt)
**题意：**   
给出一个二叉树，返回整个树的倾斜    
单个树结点的倾斜定义：左边子树的结点和与右边子树结点和的差的绝对值。空节点的倾斜为0   
整个树的倾斜定义：每个结点倾斜的和    


**思考：**  
这种一般可以使用递归来完成。  
首先需要编写一个递归方法，可以获取某个结点的所有子结点和。   
同在递归的方法中，我们可以获取左右子树的和，然后得到该结点的倾斜。同时用一个全局变量来累加单个结合的倾斜，方便最后得到树的倾斜。

**优化:**   


## [Solution](https://leetcode.com/articles/binary-tree-tilt/)
**MySolution：**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

public class Solution {
    int tilt=0;
    public int findTilt(TreeNode root) {
        traverse(root);
        return tilt;
    }
    public int traverse(TreeNode root) {
        if(root==null )
            return 0;
        int left=traverse(root.left);
        int right=traverse(root.right);
        tilt+=Math.abs(left-right);
        return left+right+root.val;
    }
}
```

**Better Solution：**  
