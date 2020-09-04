# 接雨水

[链接](https://leetcode-cn.com/problems/trapping-rain-water/)

给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

图转载自leetcode

这也是一道经典老题了。之前见过腾讯笔试有一道高楼眺望的题，相当类似。

解法也不少，最简单的当然是暴力法。

## 暴力法

直接遍历数组，对于数组中的每个元素，我们找出下雨后水能达到的最高位置，等于两边最大高度的较小值减去当前高度的值。需要两个循环，复杂度O(n^2)

```java
public class Solution {
    public int trap(int[] height) {
        int ans=0;
        for (int i = 1; i < height.length-1; i++) {
            int maxLeft =0, maxRight =0;
            //向左找最大值
            for (int j = i; j >=0; j--) {
                maxLeft=Math.max(maxLeft,height[j]);
            }
            //反之
            for (int j = i; j < height.length; j++) {
                maxRight=Math.max(maxRight,height[j]);
            }
            //最大值中较小的那个-当前元素就是接的雨水量
            ans+=Math.min(maxLeft,maxRight)-height[i];
        }
        return ans;
    }
}
```
可以说是极致的慢，88ms，堪称独一档。

## 单调栈做法

来自甜姨的思路，[题解链接](https://leetcode-cn.com/problems/trapping-rain-water/solution/dan-diao-zhan-jie-jue-jie-yu-shui-wen-ti-by-sweeti/)

维护一个单调栈，计算接雨水的量，在暴力法的基础上减少的重复遍历的时间。

```java
public class Solution {
    public int trap(int[] height) {
        if (height.length == 0) {
            return 0;
        }
        Stack<Integer> monnostack = new Stack<>();
        int ans = 0;
        for (int i = 0; i < height.length; i++) {
            while (!monnostack.isEmpty() && height[monnostack.peek()] < height[i]) {
                int now = monnostack.pop();
                //重复元素就直接出栈
                while (monnostack.size() != 0 && height[monnostack.peek()] == height[now]) {
                    monnostack.pop();
                }
                if (monnostack.size() != 0) {
                    //左柱子坐标
                    int maxleft = monnostack.peek();
                    //左右柱子高度的差值再与坐标差值相乘
                    ans += (Math.min(height[maxleft], height[i]) - height[now]) * (i - maxleft - 1);
                }
            }
            monnostack.push(i);
        }
        return ans;
    }
}
```
4ms 好了不少

## 双指针做法

应该是参考官方题解做的。
```java
class Solution {
    public int trap(int[] height) {
        int ans=0,left=0,right=height.length-1;
        int left_max=0,right_max=0;
        while (left<right)
        {
            if (height[left]<height[right])
            {
                if (height[left]>=left_max)
                    left_max=height[left];
                else
                    ans+=left_max-height[left];
                left++;
            }
            else
            {
                if (height[right]>=right_max)
                {
                    right_max=height[right];
                }
                else
                    ans+=right_max-height[right];
                right--;
            }
        }
            return ans;
}
}
```
1ms，是最快的。
