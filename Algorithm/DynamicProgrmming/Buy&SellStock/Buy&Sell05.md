# 买卖股票的最佳时机 III
给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 **两笔** 交易。  

**注意:** 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**
```
输入: [3,3,5,0,0,3,1,4]
输出: 6
解释: 在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
     随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。
```
# 解题思路
## 动态规划
* 整体思路：参考Buy&Sell02  
* 不同点：对交易次数做了限制，最多进行两次交易。
### 第一步：定义状态
由于对次数做了限制，所以除了时间状态和持有状态，还需要加上次数状态K，为了方便初始化，我们加入0次交易作为base case，0次交易的收益为0；那么就有：
```
dp[i][j][k]
i：表示第i天；
j：表示是否持有股票，0表示未持有，1表示持有；
k：表示最多允许交易k次
那么dp[i][j][k]就表示在第i天，在持有(或者未持有)股票且最多允许k次交易的情况下的最大收益。
```
### 第二步：状态转移方程
**关于交易次数的问题：**  
我们把买入当作交易的开始，卖出当作交易的结束，所以每次**当且仅当**我们买入时，交易次数会+1，故允许的交易次数k会+1。
1. 假如第i天未持有股票，且最多允许交易k次：
   1. 第i-1天未持有股票，且最多允许交易k次；
   2. 第i-1天持有股票，且最多允许交易k次，在第i天卖出；
   ```
   dp[i][0][k] = Math.max(
       //第i-1天未持有股票，且最多允许交易k次；
       dp[i-1][0][k],
       //第i-1天持有股票，且最多允许交易k次，第i天卖出；
       dp[i-1][1][k] + prices[i]
   )
   ```
2. 假如第i天持有股票，且最多允许交易k次：
   1. 第i-1天持有股票，且最多允许交易k次；
   2. 第i-1天未持有股票，且最多允许交易k-1次，(第i天买入了股票**说明第i天是新一轮交易的开始**)。
   ```
   dp[i][1][k] = Math.max(
       //第i-1天就持有股票，且最多允许交易k次；
       dp[i-1][1][k],
       //第i-1天未持有股票，且最多允许交易k-1次。
       dp[i-1][0][k-1] - prices[i]
   );
   ```
### 第三步：初始化
* 首先，考虑交易次数为0(即没有发生过交易)的情况，在该情况下，是不可能持有股票的，我们将其设置为Integer.min；
* 其次，对每种情况的第一天都进行初始化。
### 第四步：考虑输出
返回 最后一天 允许两次交易 且未持有股票的情况
```
return dp[length-1][2][0]
```
### java代码
```java
class Solution {
    public int maxProfit(int[] prices) {
        int length= prices.length;
        if (length < 2) {
            return 0;
        }
        int K = 2+1;
        int dp [][][] = new int [length][K][2];
        for (int i = 1; i < dp.length; i++) {
            dp[i][0][1] = Integer.MIN_VALUE;
        }
        for (int i = 0 ; i < length ; i++) {
            for(int k = 1 ; k < K ; k++) {
                if ( i  == 0 ) {
                    dp[0][k][0] = 0;
                    dp[0][k][1] = -prices[i];
                    continue;
                }
                //今天持有股票 1.昨天就未持有 2.昨天持有 今天卖出
                //今天未持有股票 1.昨天就持有 2.昨天未持有 今天买入
                dp[i][k][0] = Math.max(dp[i-1][k][0],dp[i-1][k][1] + prices[i]);
                dp[i][k][1] = Math.max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i]);
            }
        }
        return dp[length-1][2][0];
    }
}
```