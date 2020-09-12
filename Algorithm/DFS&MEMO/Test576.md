# 出界的路径数
给定一个 m × n 的网格和一个球。球的起始坐标为 (i,j) ，你可以将球移到相邻的单元格内，或者往上、下、左、右四个方向上移动使球穿过网格边界。但是，你**最多**可以移动 **N** 次。找出可以将球移出边界的路径数量。答案可能非常大，返回 结果 mod 109 + 7 的值。

**示例 1：**
```
输入: m = 2, n = 2, N = 2, i = 0, j = 0
输出: 6
```
## 解题思路
### 1. DFS
很典型的DFS问题，找出从某点(x,y)将球移除边界的路径数量，但是加上一个限制条件那就是最多可以移动N次，且可以走回头路，思考易知，在某个点上且在一定步数内将球移除边界的路径数量是一定的，我们可以考虑使用记忆化搜索，以减少重复的计算。

### java代码
```java
class Solution {
    private int m, n;
    //方向数组 DFS时使用
    int[][] dirs = {
            {0,1},
            {0,-1},
            {1,0},
            {-1,0}
    };
    //记忆化搜索+DFS?
    public int findPaths(int m, int n, int N, int i, int j) {
        this.m = m;
        this.n = n;
        //记忆数组  将所有元素的赋值为-1
        int[][][] memo = new int[m][n][N+1];
        for (int k = 0; k < memo.length; k++) {
            for (int l = 0; l < memo[0].length; l++) {
                for (int o = 0; o < memo[0][0].length; o++) {
                    memo[k][l][o] = -1;
                }
            }
        }
        return dfs(i, j, N, memo);
    }

    public int dfs(int x, int y, int N, int[][][] memo) {
        //如果出界 出界的路径数+1
        if (x < 0 || y < 0 || x >= m || y >= n)
            return 1;
        //未出界 但步数已经用完
        if (N == 0)
            return memo[x][y][N] = 0;
        //未出界 但该点已经访问过了 调用记忆数组 避免重复计算
        if (memo[x][y][N] != -1)
            return memo[x][y][N];
        
        //DFS
        int sum = 0;
        for (int[] dir : dirs) {
            int nextX = x + dir[0];
            int nextY = y + dir[1];
            sum += dfs(nextX, nextY, N-1, memo);
            sum %= (1000000000+7);
        }
        return memo[x][y][N] = sum;
    }
}
```
