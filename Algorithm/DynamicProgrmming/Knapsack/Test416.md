# 分割等和子集
给定一个只**包含正整数**的**非空**数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**注意:**
1. 每个数组中的元素不会超过 100
2. 数组的大小不会超过 200

**示例 1:**
```
输入: [1, 5, 11, 5]

输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].
```
## 解题思路
将数组分割成两个子集，且两子集中的元素之和相等，那么我们首先可以想到的是如果，数组的元素之和sum为奇数，那么无论如何该数组都不可能分割成两个相等的子集。如果是偶数，那么就意味着，我们要从数组中找到m个元素，使其之和为sum/2，这就变成了一个背包能否装满的问题。
## 动态规划
### 第一步：定义状态
dp[i][j] 使用前i个元素，是否可以恰好装满容量为j的背包。
### 第二步：思考状态转移方程
1. 选择第i个元素作为子集的一部分，当前子集恰好可以装满容量为j的背包，则dp[i][j] = dp[i-1][j-nums[i]];
2. 不选择第i个元素作为子集的一部分，使用前i-1个元素就能恰好装满容量为j的背包，则dp[i][j] = dp[i-1][j]
### 第三步：考虑初始化
1. dp[0][j]:使用前0个元素，装满容量为j的背包，当且仅当n为0时为真。
2. dp[i][0]:使用前i个元素，装满容量为0的背包，总为真。
### 第四步：考虑输出
输出的应是dp[n][sum/2]:使用数组的全部元素，其是否能恰好装满容量为sum/2的背包。
### **java代码**
```java
public boolean canPartition(int[] nums) {
    int sum = 0;
    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];
    }
    if((sum & 1) == 1) {
        return false;
    }
    sum >>= 1;
    boolean[][] dp = new boolean[nums.length+1][sum+1];
    for (int i = 0; i < dp.length; i++) {
        dp[i][0] = true;
    }
    for (int i = 1; i < dp.length; i++) {
        for (int j = 1; j < dp[0].length; j++) {
            if (j < nums[i-1])
                continue;
            dp[i][j] = dp[i-1][j-nums[i-1]] || dp[i-1][j];
        }
    }
    return dp[nums.length][sum];
}
```
### 第五步：考虑空间优化
在填表的过程中我们一直使用dp[i-1]来推导出dp[i]的值，因此是可以进行空间优化的，即去掉i这一维度，但是，与此同时，dp[i-1][j-nums[i-1]]中j也会对dp[i][j]造成影响，所以我们需要对j进行逆向遍历，消除该影响。
### **空间优化后的java代码**
```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int i = 0; i < nums.length; i++) 
            sum += nums[i];
        if ((sum & 1) == 1)
            return false;
        sum >>= 1;

        //01背包
        boolean[] dp = new boolean[sum + 1];

        dp[0] = true;
        for (int i = 0; i < nums.length; i++) {
            for(int j = sum; j >= nums[i]; j--) {
                dp[j] = dp[j] || dp[j - nums[i]];
            }
        }
        return dp[sum];
    }
}
```