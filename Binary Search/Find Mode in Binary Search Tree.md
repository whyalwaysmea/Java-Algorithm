## [Description](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)
Given a binary search tree (BST) with duplicates, find all the mode(s) (the most frequently occurred element) in the given BST.

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys less than or equal to the node's key.
- The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
- Both the left and right subtrees must also be binary search trees.

For example:
Given BST [1,null,2,2],
![BST ](http://img.blog.csdn.net/20180308162216790?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzQzNTg5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

return `[2].`

**Note:** If a tree has more than one mode, you can return them in any order.

**Follow up:** Could you do that without using any extra space? (Assume that the implicit stack space incurred due to recursion does not count).

## [Discuss]()
**题意：**   
给一个二叉搜索树，返回出现频率最多的元素。

关于[二叉搜索树](http://blog.csdn.net/u013435893/article/details/79400648)

**思考：**  
如果不考虑额外的空间，那么可以直接遍历二叉树，将对应的元素出现出现存起来。 
但是这不符合题意，更没有使用到二叉搜索树的特征了。不过这里可以先做一下，练习一下

**优化:**   
这里我们可以利用二叉搜索树的特征，中序遍历出来的结果就是有序的。 
这样我们只要比较前后两个元素是否相等，就等于统计出来了元素出现的次数，因为相同的元素肯定都是在一起的。  

用一个结点变量pre来记录上一个遍历到的结点，count 记录当前元素出现的次数，max记录出现最多的次数。  
如果pre不为空，说明当前不是第一个结点，和之前的一个结点值比较，如果相同count++，如果不等，那么就先count去比较一下max的值，记录下max，再把count重置。如果之前count和max比较的时候，count大于max，那么把list清空，把当前结点值放进去，如果count等于max，那么直接把当前值放入list。


## Solution
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
class Solution {
    Map<Integer, Integer> map = new HashMap<>();

	public int[] findMode(TreeNode root) {
		ArrayList<Integer> result = new ArrayList<Integer>();
        getValue(root);
        int maxValue = 0;
        for(Map.Entry<Integer, Integer> entry : map.entrySet()) {
        	Integer value = entry.getValue();
        	Integer key = entry.getKey();
        	maxValue = Math.max(maxValue, value);
        }
        
        for(Map.Entry<Integer, Integer> entry : map.entrySet()) {
        	Integer value = entry.getValue();
        	Integer key = entry.getKey();
        	if(value == maxValue) {
        		result.add(key);
        	}
        }
        int[] intResult = new int[result.size()];
        for (int i = 0; i < intResult.length; i++) {
        	intResult[i] = result.get(i);
		}
        return intResult;
    }
	// 通过递归，遍历二叉树，将所有的元素以及出现次数存起来
	public void getValue(TreeNode root) {
        if(root == null) {
            return ;
        }
		if(map.containsKey(root.val)) {
			Integer nums = map.get(root.val);
			map.put(root.val, ++nums);
		} else {
			map.put(root.val, 1);
		}
		getValue(root.left);
		getValue(root.right);
	}
}
```

**Better Solution：**  
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
class Solution {
	// 记录前一个结点
    private TreeNode prev;
    // 记录当前结点出现次数
    private int count = 0;
    // 记录最多的出现次数
    private int maxCount = -1;
    
    public int[] findMode(TreeNode root) {
        List<Integer> modes = new ArrayList();
        prev = root;
        inorder(root, modes);
        int[] ret = new int[modes.size()];
        for (int i = 0; i < modes.size(); i++){
		    ret[i] = modes.get(i);
        }
        return ret;
    }
    
    public void inorder(TreeNode root, List<Integer> modes) {
        if (root == null) return;
        inorder(root.left, modes);
        count = prev.val == root.val ? count + 1 : 1;
        
        // 当前结点出现次数和记录最大次数一样，则把当前结点放入
        if (count == maxCount) {
            modes.add(root.val);
        } else if (count > maxCount) {
	        // 如果当前结点出现次数大于了记录的最大次数，则清空存放的列表，再将当前结点值存入，并重新记录最大的出现次数
            modes.clear();
            modes.add(root.val);
            maxCount = count;
        }
        
        prev = root;
        inorder(root.right, modes);
    }
}
```