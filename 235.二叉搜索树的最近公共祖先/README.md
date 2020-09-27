# 二叉搜索树的最近公共祖先

前几天字节的面试真是。。。尴尬极了，第一次手撕代码，其实是见过的题，但是那题当初用递归做法效率非常高，双100%，就没去思考其他解法了，官方题解也没给迭代解法。结果笔试一紧张，别说迭代解法想不出来了，递归解法都写出了bug。
不过也提醒我，以后二叉树的题目要多思考思考迭代解法了。

**最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”**

这题一看，还真的有点想不到怎么做，把用例树结构化，就可以发现规律。对于两个节点p,q，如果它们的值都比根节点大，那么它们都在右子树中，也就是说它们的祖先范围缩小到右子树中，左子树的情况同理。而如果两个节点一个比根节点大，一个比根节点小，那么它们的最近公共祖先就是根节点了。

这题的递归做法和迭代做法其实内容基本一致，就是换了个皮。

## 递归做法
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root.val>p.val&&root.val>q.val)
        {
            return lowestCommonAncestor(root.left,p,q);
        }
        if(root.val<p.val&&root.val<q.val)
        {
            return lowestCommonAncestor(root.right,p,q);
        }
        return root;
    }
}
```

## 迭代做法
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode ans=root;
        while (true){
            if(p.val<ans.val&&q.val< ans.val)
            {
                ans=ans.left;
            }
            else if(p.val> ans.val&&q.val>ans.val)
            {
                ans=ans.right;
            }
            else {
                break;
            }
        }
        return ans;
    }
}
    ```
