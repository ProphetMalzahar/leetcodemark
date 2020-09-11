# 组合总数Ⅲ

连续做了好多天回溯了。。。。

**找出所有相加之和为 n 的 k 个数的组合。组合中只允许含有 1 - *9 的正整数，并且每种组合中不存在重复的数字。**

说明：

**所有数字都是正整数。**
**解集不能包含重复的组合。 **

又是回溯，又是回溯，又是回溯。

可以参考[组合](https://github.com/ProphetMalzahar/leetcodemark/tree/master/%E7%BB%84%E5%90%88)、[全排列](https://github.com/ProphetMalzahar/leetcodemark/tree/master/%E5%85%A8%E6%8E%92%E5%88%97)、
```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> lists=new ArrayList<>();
        backtrack(lists,new ArrayList<>(),n,k,1);
        return lists;
    }
    public void backtrack(List<List<Integer>> lists,List<Integer> list,int n,int k,int start){
        if (n==0&&k==list.size()){
            lists.add(new ArrayList<>(list));
            return;
        }
        if (n<=0){
            return;
        }
        for (int i = start; i < 10; i++) {
            list.add(i);
            backtrack(lists,list,n-i,k,i+1);
            list.remove(list.size()-1);
        }
    }
}
```
