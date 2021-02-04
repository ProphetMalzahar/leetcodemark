# 子数组最大平均数

给定 n 个整数，找出平均数最大且长度为 k 的连续子数组，并输出该最大平均数。

示例：

```js
输入：[1,12,-5,-6,50,3], k = 4
输出：12.75
解释：最大平均数 (12-5-6+50)/4 = 51/4 = 12.75
```

很easy的一道滑动窗口题目。维护一个长度为k的滑动窗口，向右移动不断丢弃最左的元素，加入最右的元素，同时和最大值作比较。值得注意的是max_count需要另设一个变量，因为要保存前一个滑动窗口的值。

```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        if(nums.length<k)
        {
            return 0;
        }
        int left=0,right=k-1,maxCount=0,ansCount=0;
        for(int i=0;i<k;i++)
        {
            maxCount+=nums[i];
        }
        ansCount=maxCount;
        while(right<nums.length-1)
        {
            ansCount=ansCount-nums[left]+nums[right+1];
            maxCount=Math.max(maxCount,ansCount);
            left++;
            right++;
        }
        return (double)maxCount/k;
    }
}
```

