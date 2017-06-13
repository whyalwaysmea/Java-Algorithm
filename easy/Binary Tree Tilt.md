## [Description](https://leetcode.com/problems/binary-tree-tilt/#/description)
Given a binary tree, return the tilt of the whole tree.

The tilt of a tree node is defined as the absolute difference between the sum of all left subtree node values and the sum of all right subtree node values. Null node has tilt 0.

The tilt of the whole tree is defined as the sum of all nodes' tilt.

**Example:**
![Example](http://upload-images.jianshu.io/upload_images/293077-2772cc3c58088198.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## [Discuss](https://discuss.leetcode.com/category/720/binary-tree-tilt)
**题意：**   


**思考：**  

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
