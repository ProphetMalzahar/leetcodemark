# 递归

这题而言，递归是很好想的方法。对每一个节点分别递归左子节点，右子节点，如果左/右节点不符合左小右大的特性，就直接结束递归，如果递归到节点为null，就返回true。
当然，这题的样例是个大坑，有个Integer.MAX_VALUE的极端值，如果定义的中间变量是int类型，就会越界，所以要用long类型或者double类型。


# 中序遍历

中序遍历需要利用到二叉搜索树的特性，**也就是二叉搜索树的中序遍历结果是一个递增的序列**。我们可以做一个中序遍历，如果有元素小于之前的元素，则可以直接返回false。
如果可以完成整个树的遍历，则返回true。

```java
class Solution {
    long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root != null) {
            if (!isValidBST(root.left)) {
                return false;
            }
            if (root.val <= pre) {
                return false;
            }
            pre = root.val;
            if (!isValidBST(root.right)) {
                return false;
            }
        }
        return true;
    }
}
```
