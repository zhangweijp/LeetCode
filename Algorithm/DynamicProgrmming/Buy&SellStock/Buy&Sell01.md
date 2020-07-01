# 买卖股票的最佳时机
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票。  
**示例 1:**
```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```
## 解题思路
遍历数组，遍历的同时记录当前的股票的最高价格、最低价格和最大收益，完成遍历时即可算出最大收益。
```java
class Solution{
    int maxProfit(int[] prices) {
        if (prices.length < 2)
            return 0;
        int max = prices[0];
        int min = prices[0];
        int result = 0;
        for(int i = 1;i < prices.length; i++) {
            if(prices[i] < max) {
                if(prices[i] < min)
                    min = prices[i]; 
                continue;
            }
            max = prices[i];
            result = Math.max(result,max - min);
        }
        return result;
    }
}
```