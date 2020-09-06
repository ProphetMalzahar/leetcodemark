# 无重复字符的最长子串

这题对我还是有蛮重要意义的，四月初的时候，面了阿里钉钉，那还是人生中第一次面试，数据结构、网络原理、JVM都掌握得很不好，项目也没什么技术含量，结果自然是非常失败了。啥都不会，面试算是够丢人了。😂
    
当时面试考的就是这道题。给定一个字符串，请你找出其中不含有重复字符的 最长子串。也不算是没做出来，但是用的HashSet，效率很低，面试官想让我优化，我当时没想到数组。而且要返回回文子串，我返回的总长度，面试环境下也有点紧张，想不到改法。

但是还是非常感谢那个面试官，即使我水平那么低，他还是非常耐心地跟我一一解释，鼓励我再学习、多尝试，秋招再投阿里，甚至还交换了微信，给了我一些指导。这也给了我后面努力学习的动力，才有后来去实习，到现在拿到保底offer的后续。


## 滑动窗口
维护一个最长的窗口，保证窗口中无重复字符
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // 每一个字符上一次出现的位置
        int[] last = new int[128];
        Arrays.fill(last,-1);
        int n = s.length();
        int len = 0;
        // 窗口左位置
        int start = 0;
        for(int i = 0; i < n; i++) {
            int ch=s.charAt(i);
            start=Math.max(last[ch]+1,start);
            len=Math.max(i-start+1,len);
            last[ch]=i;
        }
        return len;
    }
}
```
稍微改改可以变成那位面试官需要的结果:
```java
    public static String lengthOfLongestSubstring(String s) {
        // 记录字符上一次出现的位置
        int[] last = new int[128];
        Arrays.fill(last,-1);
        int n = s.length();
        int len = 0;
        // 窗口开始位置
        int left=0;
        int start = 0;
        for(int i = 0; i < n; i++) {
            int ch=s.charAt(i);
            start=Math.max(last[ch]+1,start);
            if (len<i-start+1)
            {
                left=start;
            }
            len=Math.max(i-start+1,len);
            last[ch]=i;
        }
        return s.substring(left,left+len);
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
来自评论区https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/comments/534978
```java
class Solution {
  	int start = 0;
	public int lengthOfLongestSubstring(String s) {
		if (s == null || s.length() == 0) {
			return 0;
		}
		char[] array = s.toCharArray();
		int[] dp = new int[array.length];
		dp[0] = 1;
		for (int i = 1; i < array.length; i++) {
			if (hasCommonChar(array, i)) {
				dp[i] = i - start;
			} else {
				dp[i] = dp[i - 1] + 1;
			}
		}

		int result = dp[0];
		for (int i = 1; i < dp.length; i++) {
			if (result < dp[i]) {
				result = dp[i];
			}
		}
		return result;

	}

	private boolean hasCommonChar(char[] array, int i) {
		boolean flag = false;
	
		for (int j = start; j < i; j++) {
			if (array[j] == array[i]) {
				flag = true;
				start = j;
			}
		}
		return flag;
	}
}
```
