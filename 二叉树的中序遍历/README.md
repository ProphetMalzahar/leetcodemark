# 二叉树的中序遍历

这是非常基础也经典的问题，课本上写的是递归做法，非常简单易懂。

## 递归做法
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
    List<Integer> list=new ArrayList<>();
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root==null)
        {
            return list;
        }
        inorderTraversal(root.left);
        list.add(root.val);
        inorderTraversal(root.right);
        return list;
    }
}
```

## 迭代做法
迭代做法相比递归做法通常效率比较高，本题由于数据量不大，递归做法也是0ms，可以击败100%，但在数据量大的情况下递归会有较多的冗余。
比如求斐波那契数列的问题中，使用迭代做法就会节省大量的时间。
而这题利用栈先进后出的特点巧妙处理了中序的添加顺序。当然，由于要使用一个栈，效率反而较低，1ms,击败46%

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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode now = root;
        while (now != null || !stack.isEmpty()) {
            if (now != null) {
                stack.push(now);
                now = now.left;
            } else {
                now = stack.pop();
                list.add(now.val);
                now = now.right;
            }
        }
        return list;
    }
}
```
