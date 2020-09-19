# 通配符匹配
给定一个字符串 (s) 和一个字符模式 (p) ，实现一个支持 '?' 和 '*' 的通配符匹配。
```
'?' 可以匹配任何单个字符。
'*' 可以匹配任意字符串（包括空字符串）。
```
两个字符串完全匹配才算匹配成功。
**说明:**
- s 可能为空，且只包含从 a-z 的小写字母。
- p 可能为空，且只包含从 a-z 的小写字母，以及字符 ? 和 *。
**示例 1:**
```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```
**示例 2:**
```
输入:
s = "acdcb"
p = "a*c?b"
输出: false
```
## 解题思路
对于这种字符串的匹配的问题，优先想到用动态规划来解决。
## 动态规划
我们依然沿用编辑字符串时所用的动规思路，从头到尾开始对两个字符串进行匹配。
### 第一步：定义状态
我们尝试用p去匹配s，首先定义一个DPTable，boolean dp[p.length()+1][s.length()+1]；则dp[i][j]就表示p[0-i]能否匹配s[0-j]
### 第二步：状态转移方程
我们首先一般情况即chsP[i-1]和chsS[j-1]相等，再考虑特殊情况'*'和'?'；
1. chsP[i-1] == chsS[j-1]
```
   dp[i][j] = dp[i-1][j-1];
```
2. chsP[i-1] != chsS[j-1]
   1. chsP[i-1] == '*'
      1. '*'匹配空字符
      ```
      dp[i][j] = dp[i-1][j];
      ```
      2. '*'匹配非空字符
      ```
      dp[i][j] = dp[i][j-1];
      ```
   2. chsP[i-1] == '?'
   ```
   dp[i][j] = dp[i-1][j-1];
   ```
   3. else
   ```
   dp[i][j] = false;
   ```
### 第三步：考虑初始化
1. dp[0][0]：两个空字符串是可以匹配的；
```
   dp[0][0] = true;
```
2. dp[i][0]：当且仅当chsP[i-1] == '*'并且dp[i-1][0]位真的时候才为真；
```
   dp[i][0] = chsP[i-1] == '*' && dp[i-1][0];
```
3. dp[0][j]：空字符串p只能匹配空字符串
```
   dp[0][j] = false;
```
### 第四步：考虑输出
输出dp[lenP][lenS]；
### 第五步：考虑空间优化
没有较好的空间优化方案。
## java代码 
```java
class Solution {
   public boolean isMatch(String s, String p) {
      char[] chsP = p.toCharArray();
      char[] chsS = s.toCharArray();

      int len1 = chsP.length;
      int len2 = chsS.length;

      boolean[][] dp = new boolean[len1+1][len2+1];

      dp[0][0] = true;

      for (int i = 1; i <= len1; i++) {
         dp[i][0] = chsP[i-1] == '*' && dp[i-1][0];
      }

      for (int i = 1; i <= len1; i++) {
         for (int j = 1; j <= len2; j++) {
            if (chsP[i-1] == chsS[j-1] || chsP[i-1] == '?') {
               dp[i][j] = dp[i-1][j-1];
               continue;
            }
            if (chsP[i-1] == '*')
               dp[i][j] = dp[i-1][j] || dp[i][j-1];
         }
      }
      
      return dp[len1][len2];
   }
}
```