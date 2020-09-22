# 无重复字符的最长子串

经典老题，找出一个字符串中最长的回文子串。

回文的定义，举个最经典的例子就是上海自来水来自海上，abcdcba这样的形式。

容易想到的解法有两种

## 中心拓展
有点类似双指针，对每一个字符向左右拓展，如果如果左指针和右指针值相等，则继续拓展，如果不相等则记录长度。
注意在开始拓展前先排除重复字符。比如abcccba，当c为中心时，先将left左移直与中心不相等，同时增加长度，右指针同理。
效率比第二种动规做法要高很多。
```java
class Solution {
    public String longestPalindrome(String s) {
        if (s.length()==0)
        {
            return s;
        }
        int left,right,len=1,start=0,maxLen=1;
        for (int i = 0; i < s.length(); i++) {
            left=i-1;
            right=i+1;
            while (left>=0&&s.charAt(left)==s.charAt(i))
            {
                len++;
                left--;
            }
            while (right<s.length()&&s.charAt(right)==s.charAt(i))
            {
                len++;
                right++;
            }
            while (left>=0&&right<s.length()&&s.charAt(left)==s.charAt(right))
            {
                len+=2;
                left--;
                right++;
            }
            start=len>maxLen?left+1:start;
            maxLen=Math.max(maxLen,len);
            len=1;
        }
        return s.substring(start,start+maxLen);
    }
}
```
官方题解也是用的HashSet,双指针，速度要慢四倍
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // 哈希集合，记录每个字符是否出现过
        Set<Character> occ = new HashSet<Character>();
        int n = s.length();
        // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
        int rk = -1, ans = 0;
        for (int i = 0; i < n; ++i) {
            if (i != 0) {
                // 左指针向右移动一格，移除一个字符
                occ.remove(s.charAt(i - 1));
            }
            while (rk + 1 < n && !occ.contains(s.charAt(rk + 1))) {
                // 不断地移动右指针
                occ.add(s.charAt(rk + 1));
                ++rk;
            }
            // 第 i 到 rk 个字符是一个极长的无重复字符子串
            ans = Math.max(ans, rk - i + 1);
        }
        return ans;
    }
}
```
## 动态规划
很显然可以知道，一个回文串通常都是由子回文串构成的，比如abcba中就包含了bcb这一回文串，所以这可以转换成一个动态规划问题。

假设我们用dp[i][j]，表示第i到第j个字符构成的字符串是否是回文串。那么可以得出状态转移方程dp[i][j]=s.charAt(i)==s.charAt(j)&&dp[i+1][j-1]
```java
class Solution {
    public String longestPalindrome(String s) {
        int n=s.length();
        boolean[][] dp=new boolean[n][n];
        String ans= "";
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n-i; j++) {
                int k=j+i;
                //长度为1的直接设置true
                if (i==0)
                {
                    dp[j][k]=true;
                }
                else if (i==1)
                {
                    dp[j][k]=(s.charAt(j)==s.charAt(k));
                }
                else {
                    dp[j][k]=(s.charAt(k)==s.charAt(j))&&dp[j+1][k-1];
                }
                if (dp[j][k]&&i+1>ans.length())
                {
                    ans=s.substring(j,j+i+1);
                }
            }
        }
        return ans;
    }
}
```
