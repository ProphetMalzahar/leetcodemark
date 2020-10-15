# 116. 填充每个节点的下一个右侧节点指针

给定一个完美二叉树，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

struct Node {

  int val;
  
  Node *left;
  
  Node *right;
  
  Node *next;
  
}

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。

初始状态下，所有 next 指针都被设置为 NULL。


![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/15/116_sample.png)

图来自LeetCode

## 非递归做法

在每一层的时候把下一层的next连好。

到下一层，由于上一层的next已经连好，可以直接连接root.right.next = root.next.left。
```java
class Solution {
    public Node connect(Node root) {
        if(root==null)
        {
            return root;
        }
        Node node=root;
        while(root.left!=null)
        {
            Node curleft=root.left;
            while(root.next!=null)
            {
                root.left.next=root.right;
                root.right.next=root.next.left;
                root=root.next;
            }
            //处理一层的最后一个左子节点
            root.left.next=root.right;
            root=curleft;
        }
        return node;
    }
}
```
## 递归做法

递归做法相对简单。每一层连接好之后推进到下一层。
```java
class Solution {
    public Node connect(Node root) {
        if(root==null||root.left==null)
        {
            return root;
        }
        root.left.next=root.right;
        //右侧节点存在
        if (root.next!=null)
        {
            root.right.next=root.next.left;
        }
        connect(root.left);
        connect(root.right);
        return root;        
    }
}
```
