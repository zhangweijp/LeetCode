# 我能赢吗
在 "100 game" 这个游戏中，两名玩家轮流选择从 1 到 10 的任意整数，累计整数和，先使得累计整数和达到或超过 100 的玩家，即为胜者。

如果我们将游戏规则改为 “**玩家不能重复使用整数**” 呢？

例如，两个玩家可以轮流从公共整数池中抽取从 1 到 15 的整数（不放回），直到累计整数和 >= 100。

给定一个整数 maxChoosableInteger （整数池中可选择的最大数）和另一个整数 desiredTotal（累计和），判断先出手的玩家是否能稳赢 （假设两位玩家游戏时都表现最佳）？

你可以假设 maxChoosableInteger 不会大于 20， desiredTotal 不会大于 300。
**示例：**
```
输入：
maxChoosableInteger = 10
desiredTotal = 11

输出：
false

解释：
无论第一个玩家选择哪个整数，他都会失败。
第一个玩家可以选择从 1 到 10 的整数。
如果第一个玩家选择 1，那么第二个玩家只能选择从 2 到 10 的整数。
第二个玩家可以通过选择整数 10（那么累积和为 11 >= desiredTotal），从而取得胜利.
同样地，第一个玩家选择任意其他整数，第二个玩家都会赢。
```
## 解题思路
我们用状态压缩数组来记录每个整数的状态，即整数i是否被使用，0表示未使用，1表示已经使用过；在每种状态下玩家先手都有固定的结果，所以我们可以用记忆化的思想，对每种状态下的结果进行记录。两种方法来判断先手是否可以获胜，一种是本次操作就可以使得总和超过 desiredTotal，那么直接获胜，还有一种是，本次操作我不知道是否可以获胜，那么我们需要判断随手接下来的这一次操作是否会获胜，若对手有机会获胜，那么先手就一定失败，若对手不可能获胜，那么先手就一定赢。
## 状态压缩 记忆化搜索
### 第一步：定义状态
定义一维数组memo[1 << maxChoosableInteger]，数组记录着从000...000到111...111的每个状态下的先手的比赛结果。
### 第二步：思考状态转移方程
1. 本次操作就可以使得总和超过 desiredTotal
```
    memo[i] = curNum >= curDesiredTotal;
```
2. 本次操作我不知道是否可以获胜，那么我们需要判断对手接下来的这一次操作是否会获胜，若对手有机会获胜，那么先手就一定失败，若对手不可能获胜，那么先手就一定赢。
```
    memo[i] = !memo[next];
```
### java代码
```java
class Solution {
    //已知(1,n)中每一个整数只可被使用一次，那我们就需要对该整数是否使用过进行存储，
    //我们可以使用状态压缩，将每个数字是否使用过的状态记录下来，方便之后的博弈
    public boolean canIWin(int maxChoosableInteger, int desiredTotal) {
        //如果所有数字都选择完毕 还是不够 直接判失败
        if ((maxChoosableInteger + 1) * maxChoosableInteger / 2 < desiredTotal)
            return false;
        return backtrack(maxChoosableInteger, desiredTotal, new Boolean[1 << maxChoosableInteger], 0);
    }

    public boolean backtrack(int maxChoosableInteger, int desireTotal, Boolean[] memo, int state) {
        //已经计算过的状态 直接从memo数组中取出并返回
        if (memo[state] != null) 
            return memo[state];
        //遍历所有可选择的数字
        for (int i = 1; i <= maxChoosableInteger; i++) {
            //如果该数字已经被选择过 则直接跳过
            int temp = 1 << (i-1);
            if ((temp & state) != 0)
                continue;
            //判断当前是否可以获胜 这个判断语句写的非常的巧妙 如果 desiredTotal - i <= 0 为真
            //那么后面的回溯就不用执行 只有 desiredTotal > 0 时 
            //即仅凭desiredTotal和i的关系不能判断当前是否可以获胜时 我们才进行回溯
            if (desireTotal <= i || !backtrack(maxChoosableInteger, desireTotal - i, memo, state | temp))
                return memo[state] = true;
        }
        //遍历所有可用数字 都不可能获胜 那么就必输
        return memo[state] = false;
    }
}
```