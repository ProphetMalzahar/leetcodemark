# 剑指offer14-I 剪绳子

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

## 动态规划
先看动态规划做法，将一个大的问题分解成若干的小问题，是比较容易想到的思路，这题我们需要找状态转移方程。对于DP[i]，即长度为i的绳子的最长乘积应当等于**max(dp[i], max(j, dp[j]) * (i - j))**

之后就是模板化的过程了

```java
class Solution {
    public int cuttingRope(int n) {
        int[] dp=new int[n+1];
        dp[1]=1;
        for (int i = 2; i <=n ; i++) {
            for (int j = 1; j < i; j++) {
                dp[i]=Math.max(dp[i],Math.max(j,dp[j])*(i-j));
            }
        }
        return dp[n];
    }
}
```

## 数学做法
因为任意一个大于1的数字均可以分解成2^n+3^k这样的形式，而且我们可以知道2*2*2<3*3，故我们需要分解后的绳子有存在尽量多的3。如果给的数字mod3==1，我们就将3的数量-1，变成2个2，就可以解决使值最大化。

PS:好久没写居然忘了Java有POW函数，差点自己实现了个快速幂算法。
```java
class Solution {
    public int cuttingRope(int n) {
        if(n <= 3) return n - 1;
        int a = n / 3, b = n % 3;
        if(b == 0) return (int)Math.pow(3, a);
        if(b == 1) return (int)Math.pow(3, a - 1) * 4;
        return (int)Math.pow(3, a) * 2;
    }
}
```
