# 递归

这题而言，递归是很好想的方法。对每一个节点分别递归左子节点，右子节点，如果左/右节点不符合左小右大的特性，就直接结束递归，如果递归到节点为null，就返回true。

```java
class Solution {
    private boolean helper(TreeNode node, Integer lower, Integer upper) {
        if (node == null) {
            return true;
        }
        int val = node.val;
        if (lower != null && val <= lower) {
            return false;
        }
        if (upper != null && val >= upper) {
            return false;
        }

        if (!helper(node.right, val, upper)) {
            return false;
        }
        return helper(node.left, lower, val);
    }

    public boolean isValidBST(TreeNode root) {
        return helper(root, null, null);
    }
}
```

时间复杂度 : O(n)，其中 n 为二叉树的节点个数。在递归调用的时候二叉树的每个节点最多被访问一次，因此时间复杂度为 O(n)。

空间复杂度 : O(n)，其中 n 为二叉树的节点个数。递归函数在递归过程中需要为每一层递归函数分配栈空间，所以这里需要额外的空间且该空间取决于递归的深度，即二叉树的高度。最坏情况下二叉树为一条链，树的高度为 n ，递归最深达到 n 层，故最坏情况下空间复杂度为 O(n) 。

# 中序遍历

中序遍历需要利用到二叉搜索树的特性，**也就是二叉搜索树的中序遍历结果是一个递增的序列**。我们可以做一个中序遍历，如果有元素小于之前的元素，则可以直接返回false。
如果可以完成整个树的遍历，则返回true。当然，这题的样例是个大坑，有个Integer.MAX_VALUE的极端值，**如果定义的中间变量是int类型，就会越界，所以要用long类型或者double类型。**

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
