# 对称二叉树

评论区这个老哥概括得非常好[这是链接](https://leetcode-cn.com/problems/symmetric-tree/comments/158689)

递归的难点在于：找到可以递归的点。

**def 函数A（左树，右树）： 左树节点值等于右树节点值 且 函数A（左树的左子树，右树的右子树），函数A（左树的右子树，右树的左子树）均为真 才返回真。**

这是我的实现：

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root==null)
            return true; 
        return ismirror(root.left,root.right);
    }
    public boolean ismirror(TreeNode left,TreeNode right)
    {
        if(left==null&&right==null)
            return true;
        if(left!=null&&right!=null)
            return left.val==right.val&&ismirror(left.left,right.right)&&ismirror(left.right,right.left);
        else return false;
    }
}
```

时间超过100%
