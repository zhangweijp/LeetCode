# 最佳买卖股票时机含冷冻期
给定一个整数数组，其中第 i 个元素代表了第 i 天的股票价格 。​

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

* 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
* 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。
 
**示例:**
```
输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```
# 解题思路
## 动态规划
* 整体思路：参考Buy&Sell02  
* 不同点:每次买入后，需进行一天的冷冻期，即交易一天才可卖出。
## Java代码
```java
class solution {
    int maxProfit(int[] prices, int fee) {
        if(prices.length < 2) {
            return 0;
        }
        int dp_hold = -prices[0];
        int dp_unhold = 0;
        
        int dp_unhold_pre = 0;

        for(int i = 1;i < prices.length; i++) {
           int temp = dp_unhold;
           dp_unhold = Math.max(dp_unhold,dp_hold+prices[i]);

           dp_hold = Math.max(dp_hold,dp_unhold_pre-prices[i]);

           dp_unhold_pre = temp;
        }
        return dp_unhold;
    }
}
```