# 前序遍历

二叉树的三种遍历也是面试的常考点，今天写一写就当做是复习了。

## 二叉树的前序遍历

### 递归
先写递归做法
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
       List<Integer> list=new ArrayList<>();
       preorder(list,root);
       return list;
    }
    public void preorder(List<Integer> list,TreeNode root)
    {
        if(root==null)
        {
            return;
        }
        list.add(root.val);
        preorder(list,root.left);
        preorder(list,root.right);
    }
}
```

### 迭代
迭代做法是面试常考的内容
通常都会用栈来完成
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
       List<Integer> list=new ArrayList<>();
       if(root==null)
       {
           return list;
       }
       Stack<TreeNode> stack=new Stack<>();
       stack.push(root);
       while(!stack.isEmpty())
       {
           TreeNode cur=stack.pop();
           list.add(cur.val);
           if(cur.right!=null)
           {
               stack.push(cur.right);
           }
           if(cur.left!=null)
           {
               stack.push(cur.left);
           }
       }
       return list;
    }
}
```

## 后序遍历

### 递归
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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> list=new ArrayList<>();
        postorder(list,root);
        return list;
    }
    public void postorder(List<Integer> list,TreeNode root)
    {
        if(root==null)
        {
            return;
        }
        postorder(list,root.left);
        postorder(list,root.right);
        list.add(root.val);
    }
}
```
### 迭代
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> list=new ArrayList<>();
        if(root==null)
        {
            return list;
        }
        //记录上一个节点
        TreeNode p=null;
        Stack<TreeNode> stack=new Stack<>();
        TreeNode cur=root;
        while(cur!=null||!stack.isEmpty())
        {
            while(cur!=null)
            {
                stack.push(cur);
                cur=cur.left;
            }
            cur=stack.peek();
            //为右子树
            if(cur.right==null||cur.right==p)
            {
                list.add(cur.val);
                stack.pop();
                p=cur;
                cur=null;
            }
            else{
                cur=cur.right;
            }
        }
        return list;
    }
}
```
