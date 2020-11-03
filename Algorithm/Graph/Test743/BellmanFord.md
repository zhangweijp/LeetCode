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

### java代码
```java
public int bellmanFord(int[][] times, int N, int K) {
    int[] dis = new int[N+1];

    Arrays.fill(dis, -1);

    dis[K] = 0;

    for (int i = 1; i <= N - 1; i++) {
        for (int[] time : times) {
            int src = time[0];
            int des = time[1];
            int v = time[2];

            if (dis[src] == -1)
                continue;

            if (dis[des] == -1) {
                dis[des] = v + dis[src];
                continue;
            }
            dis[des] = Math.min(dis[des], v + dis[src]);
        }
    }

    int max = 0;
    for (int i = 1; i <= N; i++) {
        if (dis[i] == -1)
            return -1;
        max = Math.max(dis[i], max);
    }
    return max;
}
```