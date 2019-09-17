---
layout: post
title: "LeetCode 解题笔记"
subtitle: ''
author: "Liuyun"
header-style: text
tags:
  - LeetCode
---
LeetCode刷题笔记 - 回文串相关：

给定字符串中的字符能构成的最长回文串：

https://leetcode-cn.com/problems/longest-palindrome/submissions/

遍历字符串，若有偶数个相同字符（2n)，直接加上2n，有大于1的奇数字符（2n +1），加上2n，维护一个标志位flag，表示不是全为偶数个字符

```java
class Solution {
    public int longestPalindrome(String s) {
        int[] count = new int[128];
        for(char c:s.toCharArray()){
            count[c]++;
        }
        int sum = 0;boolean flag = false;
        for(int i = 0 ;i < count.length;i ++){
            if(count[i] > 1){
                if((count[i] & 1) == 0){//偶数个该字符
                    sum += count[i];
                }
                else{//大于1的奇数个字符
                    flag = true;
                    sum += (count[i] - 1);
                }
            }
            else{
                if(count[i] == 1){//只有1个该字符
                    flag = true;
                }
            }
        }
        return flag?(sum + 1):sum;//有奇数个字符出现则再加1，表示其可作为最中间的字符
    }
}
```

https://leetcode-cn.com/problems/longest-palindromic-substring/

最长回文子串：

考虑从每个i位置从两边延伸能够达到的满足回文子串的最大长度，并在过程中维护最大长度的start、end索引。

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s == null || s.length() == 0){
            return "";
        }
        int start = 0;int end =0;
        for(int i = 0;i < s.length();i ++){
            int len1 = expand(s,i,i);
            int len2 = expand(s,i,i + 1);
            int len = Math.max(len1,len2);
            if(len > (end - start + 1)){
                start = i - (len - 1) / 2;//len为奇数情况比len为偶数情况i走的步数少1
                end = i + len / 2;//len为奇数情况和len为偶数情况j走的步数相同
            } 
        }
        return s.substring(start,end + 1);
    }
    public int expand(String s,int i ,int j){
        while(i > -1 && j < s.length() && s.charAt(i) == s.charAt(j)){
            i--;j++;
        }
        return j - i - 1;//i、j不包含在回文子串中，所以为j - i - 2 + 1
    }
}
```

