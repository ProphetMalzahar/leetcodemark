**这两题比较类似，今天刚好刷到了，就一起解决了。**

## 岛屿数量

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

这题来自评论区mata川有个特别好的思路。类似生物中细胞感染的过程，一个细胞被感染，则周围四格的细胞也被感染。一个陆地将周围所有的陆地连成一块岛屿。也就是能感染的最大面积。
要注意已感染的细胞要做标记，防止重复计算。比较类似深度优先搜索的思路。

```java
    /**
     * 岛屿的数量。
     * 岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。
     * @param grid
     * @return
     */
    public int numIslands(char[][] grid) {
        int landnum=0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j]=='1')
                {
                    infect(grid,i,j);
                    landnum++;
                }
            }
        }
        return landnum;
    }

    /**
     * 一个为1的岛屿会将周围的岛屿都感染为2
     * @param grid
     * @param i
     * @param j
     */
    public void infect(char[][] grid,int i,int j)
    {
        if(i<0||j<0||i>=grid.length||j>=grid[0].length||grid[i][j]!='1')
        {
            return;
        }
        grid[i][j]='2';
        infect(grid,i,j+1);
        infect(grid,i,j-1);
        infect(grid,i-1,j);
        infect(grid,i+1,j);
    }
```
## 岛屿的最大面积

给定一个包含了一些 0 和 1 的非空二维数组 grid 。

一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。)

这题其实跟上题相比就是题意稍有变化。做法几乎没有任何区别。

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int maxArea=0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j]==1)
                {
                    maxArea=Math.max(maxArea,DFS(grid,i,j));
                }
            }
        }
        return maxArea;
    }
    public int DFS(int[][] grid,int i,int j) {
        if (i < 0 || j < 0 || i >= grid.length || j >= grid[0].length||grid[i][j]!=1) {
            return 0;
        }
        //已访问的陆地设置为2
        grid[i][j]=2;
        int count = 1;
        count += DFS(grid, i + 1, j) + DFS(grid, i, j + 1) + DFS(grid, i - 1, j) + DFS(grid, i, j - 1);
        return count;
    }
}
```
