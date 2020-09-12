# 矩阵中的最长递增路径
给定一个整数矩阵，找出最长递增路径的长度。

对于每个单元格，你可以往上，下，左，右四个方向移动。 你不能在对角线方向上移动或移动到边界外（即不允许环绕）。
**示例 1:**
```
输入: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
输出: 4 
解释: 最长递增路径为 [1, 2, 6, 9]。
```
## 解题思路
本题只要我们求出最长路径的长度，没有要求求出准确的路径是什么，所以并不算困难，对于这种遍历数组的题目，我们可以很容易的联想到各种遍历算法，BFS、DFS、topSort等等；有易知对于每个点来说，以其作为起点的最长递增路径的长度都是一定的，那么为了避免重复计算我们可以采用记忆化搜索。
### 解题方法1：DFS+记忆化搜索
使用DFS对矩阵中的每个点都进行遍历，找到其为起点的最长递增路径，并用memo数组将最长递增路径的长度记录下来。
### java代码
```java
class Solution {
    //dfs bfs
    //方向数组
    int[][] dirs = {
        {0,1},
        {0,-1},
        {1,0},
        {-1,0}
    };
    //dfs + 记忆化搜索
    public int longestIncreasingPath(int[][] matrix) {
        if (matrix.length == 0)
            return 0;
        //记忆数组
        int[][] memo = new int[matrix.length][matrix[0].length];
        int max = 0;
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                int temp = dfs(matrix, memo, i, j);
                if (temp > max)
                    max = temp;
            }
        }
        return max;
    }

    public int dfs(int[][] matrix, int[][] memo, int x, int y) {
        //如果该节点已经方访问过  就直接从memo取出
        if (memo[x][y] != 0)
            return memo[x][y];
        //一个点的长度为1
        memo[x][y] = 1;
        //对上下左右四个方向 进行遍历
        for(int[] dir : dirs) {
            int nextX = x + dir[0];
            int nextY = y + dir[1];
            //查看下一个节点是否越界
            if (!check(matrix, nextX, nextY))
                continue;
            //是否满足递增这个要求
            if (matrix[x][y] >= matrix[nextX][nextY])
                continue;
            //从上下左右四个方向中找到最长的递增路径
            memo[x][y] = Math.max(memo[x][y], dfs(matrix, memo, nextX, nextY) + 1);
        }
        return memo[x][y];
    }

    public boolean check(int[][] matrix, int x, int y) {
        return (x >= 0 && x < matrix.length) && (y >= 0 && y < matrix[0].length);
    }
}
```