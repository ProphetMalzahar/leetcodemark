# 二叉树展开为链表

给定一个二叉树，原地将它展开为一个单链表。
> https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/

这题比较好想的是先将左右子树都处理成一条链，然后找到左子树的叶子节点，将右子树插在其后。
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public void flatten(TreeNode root) {
        if (root==null)
        {
            return;
        }
        flatten(root.left);
        flatten(root.right);
        TreeNode node = root.right;
        root.right = root.left;
        root.left = null;
        while(root.right != null) {
            root = root.right;
        }
        root.right = node;        
    }
}
```

官方题解的先序遍历做法
```java
class Solution {
    public void flatten(TreeNode root) {
        List<TreeNode> list = new ArrayList<TreeNode>();
        preorderTraversal(root, list);
        int size = list.size();
        for (int i = 1; i < size; i++) {
            TreeNode prev = list.get(i - 1), curr = list.get(i);
            prev.left = null;
            prev.right = curr;
        }
    }

    public void preorderTraversal(TreeNode root, List<TreeNode> list) {
        if (root != null) {
            list.add(root);
            preorderTraversal(root.left, list);
            preorderTraversal(root.right, list);
        }
    }
}
```
