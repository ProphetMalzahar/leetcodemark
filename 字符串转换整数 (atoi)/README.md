# 字符串转换整数 (atoi)

时隔六个月再看到这题还有些泪目，六个月前我尝试了十多次，都失败了，被各种用例卡边界，折磨得痛不欲生。

其实这题就是两点，一个是处理头符号和空格，一个是越界问题。

```java
class Solution {
    public int myAtoi(String str) {
        char chs[]=str.toCharArray();
        int n=str.length();
        //去除头的空格和符号
        int index=0;
        while (index<n&&chs[index]==' ')
        {
            index++;
        }
        if (index==n)
        {
            return 0;
        }
        boolean flag=true;
        if (chs[index]=='-')
        {
            flag=false;
            index++;
        }
        else if (chs[index]=='+'){
            index++;
        }
        //不是数字
        else if (!Character.isDigit(chs[index]))
        {
            return 0;
        }
        int nums=0;
        while (index<n&&Character.isDigit(chs[index]))
        {
            //当前位置的数字
            int num=chs[index]-'0';
            //这里有个越界的陷阱,本来应该是判断(nums*10+num)的大小，但是可能会越界。
            //直接移到右边就可以避免越界问题
            if (nums>(Integer.MAX_VALUE-num)/10)
            {
                return flag?Integer.MAX_VALUE:Integer.MIN_VALUE;
            }
            nums=nums*10+num;
            index++;
        }
        return flag?nums:-nums;
    }
}
```


评论区看到的Python一行解法
[链接](https://leetcode-cn.com/problems/string-to-integer-atoi/comments/70554)
```python
class Solution:
    def myAtoi(self, s: str) -> int:
        return max(min(int(*re.findall('^[\+\-]?\d+', s.lstrip())), 2**31 - 1), -2**31)
```
