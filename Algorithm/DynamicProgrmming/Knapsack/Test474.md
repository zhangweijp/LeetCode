# 一和零
在计算机界中，我们总是追求用有限的资源获取最大的收益。

现在，假设你分别支配着 m 个 0 和 n 个 1。另外，还有一个仅包含 0 和 1 字符串的数组。

你的任务是使用给定的 m 个 0 和 n 个 1 ，找到能拼出存在于数组中的字符串的最大数量。每个 0 和 1 **至多被使用一次**。

**注意:**

- 给定 0 和 1 的数量都不会超过 100。
- 给定字符串数组的长度不会超过 600。
## 解题思路
题目中的“最”字就提醒我们本题可以使用动态规划来解决，仔细阅读题目后，发现本题是比较经典的背包问题。
## 动态规划
### 第一步：定义状态
dp[i][j][k]:使用数组中的前i个物品，容量为m、n的背包可以存放的物品的最大数量。
### 第二步：思考状态转移方程
1. 若选择第i个元素放入背包：dp[i][j][k] = dp[i-1][j-zeros][k-ones]
2. 不选择第i个元素：dp[i][j][k] = dp[i-1][j][k]
### 第三步：考虑初始化
    dp[0][j][k] = 0;
    dp[i][0][0] = 0;
### 第四步：考虑输出
    dp[strs.length][m][n];
### java代码
```java
class Solution {
    //背包问题 物品为字符串 物品有两个维度
    public int findMaxForm(String[] strs, int m, int n) {
        int[][][] dp = new int[strs.length+1][m+1][n+1];
        int max = 0;
        for (int i = 1; i <= strs.length; i++) {
            int[] temp = getNums(strs[i-1]);
            for (int j = 0; j <= m; j++) {
                for (int k = 0; k <= n; k++) {
                    dp[i][j][k] = dp[i-1][j][k];
                    if (j >= temp[0] && k >= temp[1] && dp[i-1][j-temp[0]][k-temp[1]] + 1 > dp[i][j][k])
                        dp[i][j][k] = dp[i-1][j-temp[0]][k-temp[1]] + 1;
                    if (dp[i][j][k] > max)
                        max = dp[i][j][k];
                }
            }
        }
        return max;
    }

    public int[] getNums(String s) {
        int[] res = new int[2];
        char[] chs = s.toCharArray();
        for (char ch : chs) {
            res[ch - '0']++;
        }
        return res;
    }
}
```
### 第五步：考虑空间优化
单看 i 这个维度，dp[i]只由dp[i-1]推导而来，那么我们便可以将 i 这一维度进行优化，再看j和k，dp[j-zeros][k-ones]，若仍然按照正序进行遍历，则会对dp的值造成影响，所以我们要逆序遍历j和k消除影响。
### 空间优化后的java代码
```java
class Solution {
    //空间优化后的dp
    //背包问题 物品为字符串 物品有两个维度
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m+1][n+1];
        for (int i = 0; i < strs.length; i++) {
            int[] temp = getNums(strs[i]);
            for (int j = m; j >= temp[0]; j--) {
                for (int k = n; k >= temp[1]; k--) {
                    dp[j][k] = Math.max(dp[j][k], 1 + dp[j-temp[0]][k-temp[1]]);
                }
            }
        }
        return dp[m][n];
    }

    public int[] getNums(String s) {
        int[] res = new int[2];
        char[] chs = s.toCharArray();
        for (char ch : chs) {
            res[ch - '0']++;
        }
        return res;
    }
}
```
