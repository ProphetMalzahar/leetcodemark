# 翻转字符串里的最长单词

**给定一个字符串，逐个翻转字符串中的每个单词。**

题目：

示例 1：

输入: "the sky is blue"

输出: "blue is sky the"

示例 2：

输入: "  hello world!  "

输出: "world! hello"

解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。

示例 3：

输入: "a good   example"

输出: "example good a"

解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

看到这题第一个想到的就是栈的处理，细想了一下，感觉可能有点复杂，结果没想到其实是很不错的解法（调用API除外）。
之后一直在尝试双指针从后向前移动的做法，奈何中间移动的空格处理得不太好，一直出错，干脆放弃了。

## 调用API
毕竟是Java自带的去除空格、根据空格分隔的方法，效率还是比较高的。
```java
class Solution {
    public String reverseWords(String s) {
        s=s.trim();
        String[] strings=s.split(" ");
        StringBuilder sb=new StringBuilder();
        for (int i = strings.length-1; i >0 ; i--) {
            if (strings[i].length()!=0)
                sb.append(strings[i]).append(" ");
        }
        sb.append(strings[0]);
        return sb.toString().trim();
    }
}
```

## 栈做法
利用栈先入后出的思想，注意要要去除最后的0
```java
class Solution {
    public String reverseWords(String s) {
        int left=0,right=s.length()-1;
        //去除前空格
        while (left<=right&&s.charAt(left)==' ')
        {
            left++;
        }
        while (left<=right&&s.charAt(right)==' ')
        {
            right--;
        }
        Stack<String> stack=new Stack<>();
        StringBuilder word=new StringBuilder();
        while (left<=right) {
            char ch = s.charAt(left);
            //入栈一个字符
            if (word.length() != 0 && (ch == ' ')) {
                stack.push(word.toString());
                word.setLength(0);
            } else if (ch != ' ') {
                word.append(ch);
            }
            left++;
        }
        //最后再入栈一次
        stack.push(word.toString());
        StringBuilder sb=new StringBuilder();
        while (!stack.isEmpty())
        {
            sb.append(stack.pop()).append(" ");
        }
        return sb.substring(0,sb.length()-1);
    }
}
```
