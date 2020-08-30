# 转字符串中的单词


**题目：给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。**

示例：

输入："Let's take LeetCode contest"

输出："s'teL ekat edoCteeL tsetnoc"

## 1、直接利用StringBuilder的reverse

要注意后面的空格
```java
class Solution {
    public String reverseWords(String s) {
        if (s.length()==0) {
            return s;
        }
        String[] strings=s.split(" ");
        StringBuilder sb=new StringBuilder();
        for (int i = 0; i < strings.length-1; i++) {
            sb.append(reversestr(strings[i])).append(" ");
        }
        sb.append(reversestr(strings[strings.length-1]));
        return sb.toString();
    }
    public String reversestr(String s)
    {
        return new StringBuilder(s).reverse().toString();
    }
}
```

## 2、差不多的思路，写一个逆转的函数，转成字符数组，根据空格分隔，然后逐步调用。


```java
class Solution {
    public String reverseWords(String s) {
        char[] c = s.toCharArray();
        for(int i = 0;i < c.length;i++){
            if(c[i] == ' '){
                continue; 
            }
            int begin = i;
            while(i < c.length && c[i] != ' '){
                i++;
            }
            int end = i-1;
            reverse(c,begin,end);
        }
        return String.valueOf(c);
    }

    public void reverse(char[] c,int begin,int end){
        while(begin < end){
            char temp = c[begin];
            c[begin] = c[end];
            c[end] = temp;
            begin++;
            end--;
        }
    }
}
```

两个复杂度都差不多。
