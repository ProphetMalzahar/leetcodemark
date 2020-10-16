#  有序数组的平方

给定一个按非递减顺序排序的整数数组 A，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。

 

示例 1：

**输入：[-4,-1,0,3,10]**

**输出：[0,1,9,16,100]**

示例 2：

**输入：[-7,-3,2,3,11]**

**输出：[4,9,9,49,121]**

这题还是非常easy的，首先显而易见的可以通过排序法暴力解决，不过复杂度就是nlogn。

可以使用双指针做法，从后往前插入，易知一个非递减序列中绝对值最大的数不是在开头就是在最后，所以双指针分别从前后两端往中间移动,就可以得到平方的非递减序列。

## 暴力法
```java
class Solution {
    public int[] sortedSquares(int[] A) {
        int[] nums=new int[A.length];
        for(int i=0;i<A.length;i++)
        {
            nums[i]=(int)Math.pow(A[i],2);
        }
        Arrays.sort(nums);
        return nums;
    }
}
```

## 双指针做法
```java
class Solution {
    public int[] sortedSquares(int[] A) {
        int[] nums=new int[A.length];
        int i=0,j=A.length-1;
        int index=A.length-1;
        while(i<=j)
        {
            //这里用绝对值和平方判断都可以，没有区别。
            if(Math.abs(A[i])>Math.abs(A[j]))
            {
                nums[index--]= (int) Math.pow(A[i++],2);
            }
            else
            {
                nums[index--]= (int) Math.pow(A[j--],2);
            }
        }
        return nums;
    }
}
```
