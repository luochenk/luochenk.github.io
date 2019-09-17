LeetCode刷题笔记 - 回文串相关：

1.求给定字符串中的字符能构成的最长回文串：  https://leetcode-cn.com/problems/longest-palindrome/submissions/

​      遍历字符串，若有偶数个相同字符（2n)，直接加上2n，有大于1的奇数字符（2n +1），加上2n，维护一个标志位flag，表示不是全为偶数个字符

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

2.最长回文子串：https://leetcode-cn.com/problems/longest-palindromic-substring/

​       考虑从每个i位置从两边延伸能够达到的满足回文子串的最大长度，并在过程中维护最大长度的start、end索引。

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

3.回文字符串的数量：https://leetcode-cn.com/problems/palindromic-substrings/

​        中心扩展法：由每一个字符串向两边扩展。能扩展就表示找到了一个回文串。中心字符串不同则回文字符串起始位置或结束位置一定不同，由题意是不同的回文串。

```java
class Solution {
    public int countSubstrings(String s) {
        int count = 0;
        for(int i = 0;i < s.length() ;i ++){
            count += expand(s,i,i);
            count += expand(s,i,i + 1);
        }
        return count;
    }
    public int expand(String s,int i,int j){
        int count = 0;
        while(i >= 0 && j < s.length() && s.charAt(i) == s.charAt(j)){
            count++;
            i--;
            j++;
        }
        return count;
    }
}
```

​         动态规划法：以i为首，j为尾的字符串（其中i <  j)如果是回文字符串，需要满足i位置的字符等于j位置的字符，且以i+1为首，j-1为尾的字符串也是回文字符。当然有两种特殊情况：1、i == j直接就是回文字符串；2、(i + 1) == (j - 1)只需比较i位置的字符是否等于j位置的字符。

   

```java
class Solution{
     public int countSubstrings(String s){
          boolean[][] dp = new boolean[s.length()][s.length()];
          getDPValue(s,dp);
          int count = 0;
          for(int i = 0;i < s.length();i ++){
              for(int j = 0;j < s.length();j ++){
                  if(dp[i][j]){
                      count++;
                  }
              }
          }
          return count;
     }
     public void getDPValue(String s,boolean[][] dp){
          for(int j = 0;j < s.length();j++){
              for(int i = j;i >= 0;i--){
                  if(i == j){
                     dp[i][j] = true;
                  }
                  else{
                     dp[i][j] = s.charAt(i) == s.charAt(j) &&(j == (i + 1) || dp[i + 1][j - 1]);
                  }
              }
          }
     }
}
```

