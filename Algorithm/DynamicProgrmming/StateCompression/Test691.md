# 贴纸拼词
我们给出了 N 种不同类型的贴纸。每个贴纸上都有一个小写的英文单词。

你希望从自己的贴纸集合中裁剪单个字母并重新排列它们，从而拼写出给定的目标字符串 target。

如果你愿意的话，你可以不止一次地使用每一张贴纸，而且每一张贴纸的数量都是无限的。

拼出目标 target 所需的最小贴纸数量是多少？如果任务不可能，则返回 -1。

**示例 1：**   
输入：
```
["with", "example", "science"], "thehat"
```
输出：
```
3
```

**提示：**
- stickers 长度范围是 [1, 50]。
- stickers 由小写英文单词组成（不带撇号）。
- target 的长度在 [1, 15] 范围内，由小写字母组成。
- 在所有的测试案例中，所有的单词都是从 1000 个最常见的美国英语单词中随机选取的，目标是两个随机单词的串联。
- 时间限制可能比平时更具挑战性。预计 50 个贴纸的测试案例平均可在35ms内解决。

## 解题思路
仔细阅读题目就可以发现，本题是一个完全背包问题，但是如果我们按照传统的背包问题的思路来解决这个问题，我们会发现这个背包target有26个维度，显然传统的背包问题是不能解决这个问题的。  
提示中说到 target 的长度在 [1, 15] 范围内，那我们就可以考虑使用状态压缩的方法进行DP。

## 动态规划
### 第一步：定义状态
一维数组dp[1 << len]， 我们用二进制的角度看dp的数组下标i， 那么i就是从 000...000 开始到 111...111， 标记着target中的每个字符的使用状态， 0表示尚未使用， 1表示已经使用过了。
### 第二步：状态转移方程
```
dp[nextState] = dp[nextState] == -1 ? dp[state] + 1 : Math.min(dp[nextState], dp[state] + 1);
```
### 第三步：考虑初始化
当所有的字符全都未被选择时，那么我们我不需要从stickers中选取贴纸就可以完成任务，即
```
dp[0] = 0;
```
### 第四步：考虑输出
输出 所有字符均被选择的状态下的 所用的最少的贴纸的数目。
### 第五步：考虑空间优化
已经状态压缩过了，不可再优化。
### java代码
```
class MinStickers {
    public int minStickers(String[] stickers, String target) {
        int n = stickers.length;
        char[] chs = target.toCharArray();
        int m = chs.length;

        int[] dp = new int[1 << m];
        Arrays.fill(dp, -1);
        dp[0] = 0;

        //每个物品可以选择无限次，所以我们将stickers放在外层循环
        for (int i = 0; i < stickers.length; i++) {
            for (int state = 0; state < 1 << m; state++) {
                if (dp[state] == -1)
                    continue;
                int nextState = state;
                //将stickers放在外层循环 保证每个字符只被使用一次
                for (char c : stickers[i].toCharArray()) {
                    for (int j = 0; j < chs.length; j++) {
                        int pos = 1 << j;
                        //(nextState & pos) == 0 防止 c 的重复使用
                        if (chs[j] == c && (nextState & pos) == 0) {
                            nextState |= pos;
                            break;
                        }
                    }
                }
                dp[nextState] = dp[nextState] == -1 ? dp[state] + 1 : Math.min(dp[nextState], dp[state] + 1);
            }
        }

        return dp[dp.length - 1];
    }
}
```