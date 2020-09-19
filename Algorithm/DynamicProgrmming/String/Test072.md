# 编辑距离
给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：

- 插入一个字符
- 删除一个字符
- 替换一个字符

**示例 1：**
```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```
**示例 2：**
```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```
## 解题思路
我们了解到编辑字符串，有三种操作，一是替换，就意味着直接将word1中某个字符替换成word2中相应的字符；二是跳过，word1和word2中对应位置的两个字符相同，我们就不需要对它们进行编辑；三是删除(插入)，对应位置的两个字符若不同，我们也可以考虑将其中一个进行删除(插入)。
## 动态规划
分别从两个字符串的头部(空字符)开始遍历并比较两个字符串。
### 第一步：定义状态
dp[i][j] 表示 从 word1[0-i] 到 word2[0-j] 所使用的最少操作数。
### 第二步：状态转移方程
1. 第一种情况，两个字符chs1[i-1]与chs2[j-1]相同，我们直接跳过
   dp[i][j] = dp[i-1][j-1];
2. 第二种情况，两个字符chs1[i-1]与chs2[j-1]不相同
   1. 我们对其中一个字符进行替换，那我们只用匹配 word1 前i-1个字符和 word2 前j-1个字符，即：
   ```
        dp[i][j] = dp[i-1][j-1] + 1;
   ```
   2. 我们选择删除 word1 的第i个字符，那么就意味着我们只需匹配 word1 的前i-1个字符和 word2 的前j个字符，即：
   ```
        dp[i][j] = dp[i-1][j] + 1;
   ```
   3. 我们选择在 word1 的第i个字符后添加一个字符，使其与 word2 的第j个字符匹配，那我们就只需要匹配 word1 的前i个符和 word2 的前j-1个字符，即：
   ```
        dp[i][j] = dp[i][j-1] + 1;
   ```
因为我们我们要求出最少的操作次数，所以我们要从第二章情况中选出操作次数的最少的一个。
```
    dp[i][j] = Math.min(dp[i-1][j-1], Math.min(dp[i-1][j], dp[i][j-1])) + 1;
```
### 第三步：考虑初始化
dp[i][0] word1[0-i]与空字符串进行比较，需要操作的次数就是i，即：
```
    dp[i][0] = i;
```
dp[0][j] word2[0-j]与空字符串进行比较，需要操作的次数就是j，即：
```
    dp[0][j] = j;
```
### 第四步：考虑输出
输出的是匹配 word1 和 word2 两个完整的字符串所花费的最少操作次数，即：dp[word1.length][word2.length];
### 第五步：考虑空间优化
本题没有较好的优化方法。
### java代码
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int len1 = word1.length;
        int len2 = word2.length;

        char[] chs1 = word1.toCharArray();
        char[] chs2 = word2.toCharArray();

        int[][] dp = new int[len1+1][len2+1];

        //对dpTable 进行初始化
        for (int i = 1; i <= len1; i++)
            dp[i][0] = i;

        for (int j = 1; j <= len2; j++)
            dp[0][j] = j;
        
        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (chs[i-1] == chs2[j-1])
                    dp[i][j] = dp[i-1][j-1];
                else 
                    dp[i][j] = Math.min(dp[i-1][j-1], Math.min(dp[i-1][j], dp[i][j-1])) + 1;
            }
        }

        return dp[len1][len2];
    }
}
```