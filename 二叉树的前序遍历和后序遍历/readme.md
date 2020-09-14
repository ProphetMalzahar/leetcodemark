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
