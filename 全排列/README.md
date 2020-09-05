# 全排列

全排列是个数学中的概念，也就是将数字的不同组合列举出来。

给定一个没有重复 数字的序列，返回其所有可能的全排列。

示例:

输入: [1,2,3]

输出:

[

  [1,2,3],
  
  [1,3,2],
  
  [2,1,3],
  
  [2,3,1],
  
  [3,1,2],
  
  [3,2,1]
  
]


来源：力扣（LeetCode）

[链接](https://leetcode-cn.com/problems/permutations)

## 比较标准的回溯
```java
class Solution {
    /**
     * @param nums
     * @return
     */
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> lists=new ArrayList<>();
        int[] visited=new int[nums.length];
        backtrack(lists, new ArrayList<>(),nums,visited);
        return lists;
    }
    public void backtrack(List<List<Integer>> lists,List<Integer> list,int[] nums,int[] visited)
    {
        if (list.size()== nums.length)
        {
            lists.add(new ArrayList<>(list));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (visited[i]==1)
            {
                continue;
            }
            //减少重复访问
            visited[i]=1;
            //增加元素
            list.add(nums[i]);
            //回溯
            backtrack(lists,list,nums,visited);
            //撤销
            visited[i]=0;
            list.remove(list.size()-1);
        }
    }
}
```

    回溯算法实际上一个类似枚举的搜索尝试过程，主要是在搜索尝试过程中寻找问题的解，当发现已不满足求解条件时，就“回溯”返回，
尝试别的路径。回溯法是一种选优搜索法，按选优条件向前搜索，以达到目标。但当探索到某一步时，发现原先选择并不优或达不到目标，
就退回一步重新选择，这种走不通就退回再走的技术为回溯法，而满足回溯条件的某个状态的点称为“回溯点”。许多复杂的，规模较大的问题
都可以使用回溯法，有“通用解题方法”的美称。

**这是回溯的标准模板:**
```java
class Solution {
    static List<List<Integer>> res = new LinkedList<>();
 @
    public List<List<Integer>> permute(int[] nums) {
        LinkedList<Integer> track = new LinkedList<>();
        help(nums, track);
        return res;
    }

    public void help(int[] nums, LinkedList<Integer> track) {
        // 满足结束条件
        if (track.size() == nums.length) {
            res.add(new LinkedList<>(track));
            return;
        }
        // for 选择 in 选择列表
        for (int i = 0; i < nums.length; i++) {
            if (track.contains(nums[i])) {
                continue;
            }
            // 做选择
            track.add(nums[i]);
            // 进入下一层决策树
            help(nums, track);
            // 撤销选择
            track.removeLast();
        }
    }
}
```
[参考LeetCode评论区](https://leetcode-cn.com/problems/permutations/comments/576030)
