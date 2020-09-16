# 翻转二叉树

今天的一道简单题，就是把二叉树上的所有左右节点互换。

[链接](https://leetcode-cn.com/problems/invert-binary-tree/)

## 递归
这题数据量不大，递归就可以100% 98%
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root==null)
        {
            return null;
        }
        TreeNode node=root.left;
        root.left=root.right;
        root.right=node;
        if (root.left==null&&root.right==null)
        {
            return root;
        }
        root.left=invertTree(root.left);
        root.right=invertTree(root.right);
        return root;
    }
}
```
