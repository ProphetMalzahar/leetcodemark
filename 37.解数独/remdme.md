# 解数独
[题目链接](https://leetcode-cn.com/problems/sudoku-solver/)

编写一个程序，通过已填充的空格来解决数独问题。

一个数独的解法需遵循如下规则：

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。
空白格用 '.' 表示。


![img](http://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

**一个数独**


![img](http://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)

**答案被标成红色**


## 回溯
看了一下题意，感觉这又是一道回溯的题目，九月做了太多回溯了。

思路也是经典的回溯，每走一步，判断这一步是否合法，如果不合法，撤销这一步，结束。
```java
class Solution {
    public void solveSudoku(char[][] board) {
        backtrack(board,0,0);
    }
    //给定数独永远是9×9的形式
    int len=9;
    boolean vadid=false;
    char[] choice = {'1','2','3','4','5','6','7','8','9'};
    public void backtrack(char[][] board,int x,int y)
    {
        if (x== len-1&&y>=len)
        {
            vadid=true;
            return;
        }
        //换行
        if (y>=len){
            y%=len;
            x++;
        }
        //当前块不为空，向右走
        if (board[x][y]!='.'){
            backtrack(board,x,y+1);
        }
        else {
            for (char ch:choice)
            {
                if (isVadid(board,x,y,ch))
                {
                    board[x][y]=ch;
                    backtrack(board, x, y+1);
                    if (vadid)
                    {
                        return;
                    }
                    board[x][y]='.';
                }
            }
        }
    }
    public boolean isVadid(char[][] board,int x,int y,char ch)
    {
        //判断行和列
        for (int i = 0; i < len; i++) {
            if (board[x][i]==ch||board[i][y]==ch)
            {
                return false;
            }
        }
        //判断3*3方阵
        int X = x/3*3;
        int Y = y/3*3;
        for(int i = X;i < X+3;i++){
            for(int j = Y;j < Y+3;j++){
                if(board[i][j] == ch) {
                    return false;
                }
            }
        }
        return true;
    }
}
```
## 位运算优化做法
来自官方题解，很巧妙，借助位运算，仅使用一个整数表示每个数字是否出现过。

具体地，数 bb 的二进制表示的第 ii 位（从低到高，最低位为第 00 位）为 11，当且仅当数字 i+1i+1 已经出现过。

例如当 bb 的二进制表示为 (011000100)_2时，就表示数字 33，77，88 已经出现过。

```java
class Solution {
    private int[] line = new int[9];
    private int[] column = new int[9];
    private int[][] block = new int[3][3];
    private boolean valid = false;
    private List<int[]> spaces = new ArrayList<int[]>();

    public void solveSudoku(char[][] board) {
        for (int i = 0; i < 9; ++i) {
            for (int j = 0; j < 9; ++j) {
                if (board[i][j] == '.') {
                    spaces.add(new int[]{i, j});
                } else {
                    int digit = board[i][j] - '0' - 1;
                    flip(i, j, digit);
                }
            }
        }

        dfs(board, 0);
    }

    public void dfs(char[][] board, int pos) {
        if (pos == spaces.size()) {
            valid = true;
            return;
        }

        int[] space = spaces.get(pos);
        int i = space[0], j = space[1];
        int mask = ~(line[i] | column[j] | block[i / 3][j / 3]) & 0x1ff;
        for (; mask != 0 && !valid; mask &= (mask - 1)) {
            int digitMask = mask & (-mask);
            int digit = Integer.bitCount(digitMask - 1);
            flip(i, j, digit);
            board[i][j] = (char) (digit + '0' + 1);
            dfs(board, pos + 1);
            flip(i, j, digit);
        }
    }

    public void flip(int i, int j, int digit) {
        line[i] ^= (1 << digit);
        column[j] ^= (1 << digit);
        block[i / 3][j / 3] ^= (1 << digit);
    }
}
```
