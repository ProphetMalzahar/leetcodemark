# 寻找数组的中心索引

给你一个整数数组 nums，请编写一个能够返回数组 “中心索引” 的方法。

数组 中心索引 是数组的一个索引，其左侧所有元素相加的和等于右侧所有元素相加的和。

如果数组不存在中心索引，返回 -1 。如果数组有多个中心索引，应该返回最靠近左边的那一个。

注意：中心索引可能出现在数组的两端。


这题的关键就是找到左边值总和*2+nums[i]==sum的位置。

```java
class Solution {
    public int pivotIndex(int[] nums) {
        if(nums==null||nums.length==0)
        {
            return -1;
        }
        int sum=0;
        for(int i:nums)
        {
            sum+=i;
        }
        int left=0;
        for(int i=0;i<nums.length;i++)
        {
            if((2*left+nums[i])==sum)
            {
                return i;
            }
            left+=nums[i];
        }
        return -1;
    }
}
```
两趟遍历，复杂度O(n) 可能不是最优解。

