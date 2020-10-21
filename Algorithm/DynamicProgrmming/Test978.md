# 最长湍流子数组
当 A 的子数组 A[i], A[i+1], ..., A[j] 满足下列条件时，我们称其为湍流子数组：

- 若 i <= k < j，当 k 为奇数时， A[k] > A[k+1]，且当 k 为偶数时，A[k] < A[k+1]；
- 或 若 i <= k < j，当 k 为偶数时，A[k] > A[k+1] ，且当 k 为奇数时， A[k] < A[k+1]。  

也就是说，如果比较符号在子数组中的每个相邻元素对之间翻转，则该子数组是湍流子数组。

返回 A 的**最大**湍流子数组的长度。
## 解题思路
题目中说明要求最大湍流子数组的长度，这种求最值、最优解的问题很容易想到用动态规划解决问题！
## 动态规划
### 第一步：定义状态
dp[i][2]:
- dp[i]记录的是以第i个元素为结尾的湍流子数组的长度
- dp[i][0]记录的 是形式为 当 k 为奇数时，A[k] > A[k+1] ，且当 k 为偶数时，A[k] < A[k+1] 、以i为结尾的湍流子数组；
- dp[i][1]记录的 是形式为 当 k 为偶数时，A[k] > A[k+1] ，且当 k 为奇数时，A[k] < A[k+1] 、以i为结尾的湍流子数组。
### 第二步：思考状态转移方程
1. a[i] > a[i-1]
   1. 如果 i为奇数：dp[i][0] = dp[i-1][1] + 1;
   2. 如果 i为偶数：dp[i][1] = dp[i-1][0] + 1;
2. a[i] < a[i-1]
   1. 如果 i为奇数：dp[i][0] = dp[i-1][0] + 1;
   2. 如果 i为偶数：dp[i][1] = dp[i-1][1] + 1;
### 第三步：考虑初始化
    数组中所有元素的初始值都置为 1，因为每个元素都是长度为1的湍流子数组。
### 第四步：考虑输出
    使用max来记录，最大湍流子数组的长度。
### java代码
```java
class Solution {
    public int maxTurbulenceSize(int[] A) {
        int[][] dp = new int[A.length][2];
        for (int i = 0; i < dp.length; i++) {
            for (int j = 0; j < dp[i].length; j++) {
                dp[i][j] = 1;
            }
        }
        int max = 1;
        for (int i = 1; i < dp.length; i++) {
            //后大于前
            if (A[i] > A[i-1]) {
                //i+1为偶数
                if ((i & 1) == 1)
                    dp[i][0] = dp[i-1][0] + 1;
                //i+1为奇数
                else
                    dp[i][1] = dp[i-1][1] + 1;
            } else if (A[i] < A[i-1]) {
                //前大于后
                //i+1为偶数
                if ((i & 1) == 1)
                    dp[i][1] = dp[i-1][1] + 1;
                //i+1为奇数
                else
                    dp[i][0] = dp[i-1][0] + 1;
            }else
                continue;
        }

        for (int i = 0; i < dp.length; i++) {
            for (int j = 0; j < dp[i].length; j++) {
                if (dp[i][j] > max)
                    max = dp[i][j];
            }
        }

        return max;
    }
}
```
### 第五步：考虑空间优化
    不难看出，dp[i]仅有dp[i-1]推出，那么我们就可以对dp数组进行降维，使用两个变量来代替数组进行数据的更新。
### 空间优化后的java代码
```java
class Solution {
    public int maxTurbulenceSize(int[] A) {
        int max = 1;
        //空间优化
        int t_1 = 1;
        int t_2 = 1;
        for (int i = 1; i < A.length; i++) {
            if (A[i] > A[i-1]) {
                if ((i & 1) == 1) {
                    t_1++;
                    t_2 = 1;
                }else {
                    t_2++;
                    t_1 = 1;
                }
            } else if (A[i] < A[i-1]) {
                if ((i & 1) == 1) {
                    t_2++;
                    t_1 = 1;
                }else {
                    t_1++;
                    t_2 = 1;
                }
            } else {
                t_1 = 1;
                t_2 = 1;
                continue;
            }
            if (t_1 > max)
                max = t_1;
            if (t_2 > max)
                max = t_2;
        }
        return max;
    }
}
```