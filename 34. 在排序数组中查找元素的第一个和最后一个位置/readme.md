# 在排序数组中查找元素的第一个和最后一个位置

题目：

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 O(log n) 级别。

如果数组中不存在目标值，返回 [-1, -1]。


看到时间复杂度的要求，是比较容易联想到二分法的，需要注意边界问题，结束循环的条件。

PS：我就是忘了break，还纳闷怎么一直超时，没法pass。

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] res={-1,-1};
        if (nums.length<1)
        {
            return res;
        }
        int low=0,high= nums.length-1;
        while (low<=high)
        {
            int mid=(low+high)/2;
            if (nums[mid]==target&&(mid-1<0||nums[mid-1]!=target))
            {
                res[0]=mid;
                break;
            }
            else if (nums[mid]>=target)
            {
                high=mid-1;
            }
            else
            {
                low=mid+1;
            }
        }
        if (res[0]==-1)
        {
            return res;
        }
        low=res[0];
        high= nums.length-1;
        while (low<=high)
        {
            int mid=(low+high)/2;
            if (nums[mid]==target&&(mid+1>=nums.length||nums[mid+1]!=target))
            {
                res[1]=mid;
                break;
            }
            else if (nums[mid]<=target)
            {
                low=mid+1;
            }
            else
            {
                high=mid-1;
            }
        }
        return res;
    }
}
```
