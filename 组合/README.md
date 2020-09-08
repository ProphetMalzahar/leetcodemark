# 组合
组合和[全排列](https://github.com/ProphetMalzahar/leetcodemark/tree/master/%E5%85%A8%E6%8E%92%E5%88%97)
还是比较相似的，都是标准的回溯+剪枝。
这种题目当发现k不固定，就得想到不能用数组的遍历。回溯算法就是发现某一步不可行，就退回上一步，同时根据一些判断条件剪枝。做多了也就熟能生巧，套模板就完事了。
```java
class Solution {
    /**
     * 给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。
     * @param n
     * @param k
     * @return
     */
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> lists=new ArrayList<>();
        backtrack(lists,1,n,k,new ArrayList<>());
        return lists;
    }
    /**
     * 回溯
     * @param lists
     */
    public void backtrack(List<List<Integer>> lists,int start,int n,int k,ArrayList<Integer> list)
    {
        //剩的元素不够组合
        if (list.size()+n-start+1<k)
        {
            return;
        }
        if (list.size()==k) {
            lists.add(new ArrayList<>(list));
        }
        else {
            for (int i = start; i <=n; i++) {
                list.add(i);
                backtrack(lists,i+1,n,k,list);
                list.remove(list.size()-1);
            }
        }
    }
}
```
