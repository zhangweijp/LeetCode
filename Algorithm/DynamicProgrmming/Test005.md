# 最长回文子串
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。  
**示例 1：**  
```
    输入: "babad"
    输出: "bab"
    注意: "aba" 也是一个有效答案。
```
**示例 2：**  
```
    输入: "cbbd"
    输出: "bb"
```
## 解题思路
1. 暴力法：  
用内外两层循环（left,right），对每个子字符串都进行判断，若是回文串,则继续判断其长度是否大于最大值若是则更新最长子字符串的首尾(start,end)。
```java
    int maxLen = 0;
    int start = 0;
    int end = 1;
    for(int left = 0;left < len; left++){
        for(int right = left+2;right <= len; right++){
            if(isPalindrome(s.subString(left,right))
            ){
                if(right - left > maxLen){
                    start = left;
                    end = right;
                    maxLen = right - left;
                }
            }
        }
    }
    return s.subString(start,end);
```

2. 动态规划：  
### 第一步：定义状态
dp[i][j]表示子串s[i..j]是否为回文串；
### 第二步：思考状态转移方程
我们如何判断s[i..j]是回文串，那我们就从简单的情况开始推理；  
假如s的长度为1，那么s必为回文串，这是基础情况，继续延伸；  
假如s的长度为2，那么只有两个字符串相同的情况下即s[i]==s[j]，s才是回文串；  
假如s的长度为3，只要s[i]==s[j]，那么s就必然是回文串；  

**以上三种是基础情况,接下来就是一般规律**  
假如s的长度为n>3时，s[i]==s[j]的同时,夹在i、j之间的子字符串s[i+1..j-1]也必须得是回文串，才能使s[i..j]这个整体为回文串，那么状态转移方程就是:  
```
    dp[i][j] = s[i]==s[j] && dp[i+1][j-1];
```
### 考虑初始化
经过上述内容的讨论，只要s[i]==s[j]或者字符串长度为1时，且字符串的长度小于等于3时，字符串s[i..j]必为回文；
### 考虑输出
我们最后要输出的是什么？输出的是最长的回文子串，所以我们在判断子串为回文串之后还有其他的工作需要解决，我们通过i、j的值就可以知道当前的回文子串的长度是否超过了当前最长回文串的长度，若超过了，就更新最大回文长度并保存位置信息。
### 考虑空间优化
在填表的过程中只使用了dp[i+1][j-1]作为推导信息，很明显是可以空间优化，用一维数组就可以完成整个动态规划的过程。
```java
public class LongestPalindrome {
    public String longestPalindrome(String s) {
        //字符串长度
        int len = s.length();
        if (len==0)
            return s;
        //字符串转换成字节数组
        char[] chs = s.toCharArray();
        //最大子回文串的长度、开始位置、结束位置
        int maxLen = 0;
        int start = 0;
        int end = 0;
        boolean[][] dp = new boolean[len][len];
        //单个字符一定是回文
        for (int i = 0; i < dp.length; i++)
            dp[i][i] = true;
        for (int left = len-2; left >= 0; left--) {
            for (int right = left+1; right < len; right++) {
                if (chs[left] == chs[right]) {
                    if (right - left < 3)
                        dp[left][right] = true;
                    else
                        dp[left][right] = dp[left+1][right-1];
                }
                if (dp[left][right] && right - left > maxLen) {
                    start = left;
                    end = right;
                    maxLen = right - left;
                }
            }
        }
        return s.substring(start,end+1);
    }
}
```  
