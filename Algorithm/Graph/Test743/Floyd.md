# 网络延迟时间
有 N 个网络节点，标记为 1 到 N。

给定一个列表 times，表示信号经过有向边的传递时间。 times[i] = (u, v, w)，其中 u 是源节点，v 是目标节点， w 是一个信号从源节点传递到目标节点的时间。

现在，我们从某个节点 K 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 -1。
```
输入：times = [[2,1,1],[2,3,1],[3,4,1]], N = 4, K = 2
输出：2
```
## 解题思路
本题问的是需要多久才能使所有节点都收到信号，其实就是求信号从节点 K 出发，到遍历所有节点所花费的最短时间，也就是求 K 到每个节点的最短路径中的最大值。我们就可以使用Dijkstra、BellmanFord和Floyd等等算法来求最短路径。
## Floyd
加入共有 n (1 - n) 个节点，已知 i 到 j 之间有一条直接路径 Eij = 20， 如果此时，存在一个中间节点 k ， 使得从 i 到 k 的距离 Eik 加上 k 到 j 的距离 Ekj 之和(Eik + Ejk)小于 Eij ，那么我们就得到了比 Eij 一个更短的路径，那么同理，我们是不是也可以对 Eik 以及 Ekj 做相同的操作呢，那我们就能进一步优化 i 到 j 的最短路径，以此类推，我们就可以通过Floyd算法找到任意两点之间的最短路径。
### 算法流程
首先，我们确定中间节点 k ，遍历所有可能的起始点 i 和 终点 j， 对比 Eij 和 (Eik + Ekj) 求出最短距离。
### java代码
```java
public int floyd(int[][] times, int N, int K) {
    int[][] matrix = new int[N][N];

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            matrix[i][j] = i == j ? 0 : Integer.MAX_VALUE;
        }
    }

    for (int[] time : times)
        matrix[time[0]-1][time[1]-1] = time[2];


    //通过i、j确定一条路径的起点和终点，k作为中间节点，遍历所有的可能的k，找到最短的路径。
    for (int k = 0; k < N; k++) {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                if (matrix[i][k] == Integer.MAX_VALUE || matrix[k][j] == Integer.MAX_VALUE)
                    continue;
                if (matrix[i][k] + matrix[k][j] < matrix[i][j])
                    matrix[i][j] = matrix[i][k] + matrix[k][j];
            }
        }
    }

    int max = 0;
    for (int i = 0; i < N; i++) {
        max = Math.max(max, matrix[K-1][i]);
    }

    return max == Integer.MAX_VALUE ? -1 : max;
}
```