# 最长连续序列
前几天在忙笔试，两天都没更新，其实刷题是没停的，反倒题量比之前还要大了。

**给定一个未排序的整数数组，找出最长连续序列的长度。**

要求算法的时间复杂度为 O(n)。

**示例:**

输入: [100, 4, 200, 1, 3, 2]

输出: 4

解释: 最长连续序列是 [1, 2, 3, 4]。它的长度为 4。


## 题解：
这题麻烦就麻烦在O(n)的时间复杂度，也因为这个，所以才是hard难度。如果可以放宽到O（logn），直接做个排序，然后转换成连续最长子序列来做就行了。

前几天看到一个比较类似的面试题，那题是在巨大量数据的前提下，显然没有办法建一个对应的数组，这种题目需要想到用HashMap解决。这题也相似，设置一个HashMap,HashMap中存储的是对应数字的子序列长度。

遍历数组，如果num-1在map中存在，那么left就取其value，right同理。而当前num的子序列长度就是left+right+1，即map.get(num-1)+map.get(num+1)+1。同时要注意，修改了当前map的值之后，当前区间两端点的值需要修改。

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length==0)
        {
            return 0;
        }
        Map<Integer,Integer> map=new HashMap<>();
        int maxlen=0;
        for (int num : nums) {
            if (!map.containsKey(num)) {
                int left = map.getOrDefault(num - 1, 0);
                int right = map.getOrDefault(num + 1, 0);
                int curlen = left + right + 1;
                if (curlen > maxlen) {
                    maxlen = curlen;
                }
                map.put(num, curlen);
                map.put(num-left,curlen);
                map.put(num+right,curlen);
            }
        }
        return maxlen;
    }
}
```

这是官方题解的HashSet做法
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> num_set = new HashSet<Integer>();
        for (int num : nums) {
            num_set.add(num);
        }

        int longestStreak = 0;

        for (int num : num_set) {
            if (!num_set.contains(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                while (num_set.contains(currentNum + 1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }

        return longestStreak;
    }
}
```
