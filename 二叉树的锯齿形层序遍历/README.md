# 二叉树的锯齿形层序遍历


**给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。**

## 队列解法

这题一开始想到的就是标准的层序遍历，然后再加个flag判断遍历方向。也就是利用队列的特性，根结点入队，根结点出队，子节点入队，由此循环直至队列为空，整棵树就遍历完了。
此题需要根据flag来决定是头插还是尾插，这个也是本解法的关键点。

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        boolean flag = false;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> list = new ArrayList<>();
            while (size-- > 0) {
                TreeNode node = queue.poll();
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
                if (flag) {
                    list.add(0, node.val);
                } else {
                    list.add(node.val);
                }
            }
            flag = !flag;
            res.add(list);
        }
        return res;
    }
}
```

## 两个栈的解法


```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        Stack<TreeNode> stack = new Stack();
        Stack<TreeNode> stack2 = new Stack();
        List<List<Integer>> res = new ArrayList();
        if(root == null){
            return res;
        }
        stack.push(root);
        int i =1;
        while(!stack.isEmpty() || !stack2.isEmpty()){
            int size = i%2 == 1? stack.size(): stack2.size();
            List<Integer> list = new ArrayList();
            if(i%2==0){
                while(size-- > 0){
                    TreeNode pop = stack2.pop();
                    list.add(pop.val);
                    if(pop.right !=null){
                        stack.push(pop.right);
                    }
                    if(pop.left != null){
                        stack.push(pop.left);
                    }
                }
            }else{
                while(size-- > 0){
                    TreeNode pop = stack.pop();
                    list.add(pop.val);
                    if(pop.left != null){
                        stack2.push(pop.left);
                    }
                    if(pop.right !=null){
                        stack2.push(pop.right);
                    }
                }
            }
            res.add(list);
            i++;
        }
        return res;
    }
}
```
