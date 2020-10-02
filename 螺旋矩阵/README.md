分为两道题，不过思路大同小异
# 54.螺旋矩阵

给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

例如这个数组：

[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]

输出一个列表，元素为{1,2,3,4,5,6,7,8,9}

思路比较好想，题目给的比较明显就，就是螺旋状的遍历。华为笔试考到了这道题，不过因为当时边界值处理得不太好，导致没有100%pass，最后也没通过。

left、right、top、bottom是四个边界，判断遍历是否结束。

向右、向下移动的过程不做判断，但是向左、向上进行前需要判断，左右边界是否相撞了，上下边界是否相撞了，如果是，直接结束这一轮循环。

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> list=new ArrayList<>();
        if (matrix==null||matrix.length==0)
        {
            return list;
        }
        int left=0,right= matrix[0].length-1;
        int top=0,bot= matrix.length-1;
        while (left<=right&&bot>=top)
        {
            int i=left;
            while (i<=right)
            {
                list.add(matrix[top][i]);
                i++;
            }
            top++;
            i=top;
            while (i<=bot)
            {
                list.add(matrix[i][right]);
                i++;
            }
            right--;
            i=right;
            while (i>=left&&bot>=top)
            {
                list.add(matrix[bot][i]);
                i--;
            }
            bot--;
            i=bot;
            while (i>=top&&left<=right)
            {
                list.add(matrix[i][left]);
                i--;
            }
            left++;
        }
        return list;
    }
}
```

# 59.螺旋矩阵Ⅱ

给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

没什么好写的，和第一题思路基本一样,就是一个逆过程。

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] matrix=new int[n][n];
        int left=0,right= matrix[0].length-1;
        int top=0,bot= matrix.length-1;
        int num=1;
        while (left<=right&&top<=bot)
        {
            int i=left;
            while (i<=right)
            {
                matrix[top][i]=num++;
                i++;
            }
            top++;
            i=top;
            while (i<=bot)
            {
                matrix[i][right]=num++;
                i++;
            }
            right--;
            i=right;
            while (i>=left&&bot>=top)
            {
                matrix[bot][i]=num++;
                i--;
            }
            bot--;
            i=bot;
            while (i>=top&&left<right)
            {
                matrix[i][left]=num++;
                i--;
            }
            left++;
        }
        return matrix;
    }
}
```
