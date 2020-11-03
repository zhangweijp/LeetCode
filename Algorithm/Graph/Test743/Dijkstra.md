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
## Dijkstra
### 应用场景 
有向正权图
### 算法流程
Dijkstra算法将图中的节点分为两个集合：
- 已经访问过的节点集合  
- 可访问的节点集合  

1. 我们将出发点cur加入到已访问节点集合Visited，将其邻接的节点加入到可访问但为访问的节点集合Distance；
2. 遍历Diatance数组，找到距离cur最近的节点next，将next节点加入到Visited数组；
3. 我们再将next节点当作出发点，重复 1、2 操作，直到所有节点都遍历完毕。

### java代码
```java
public int dijkstra(int[][] times, int N, int K) {
    //邻接表 HashMap来实现
    Map<Integer, List<int[]>> adjGraph = new HashMap<>();

    for (int[] time : times) {
        if (!adjGraph.containsKey(time[0]))
            adjGraph.put(time[0], new ArrayList<>());
        adjGraph.get(time[0]).add(new int[]{time[1], time[2]});
    }

    //新建Distance[] 保存K到每个节点的最短路径长度
    int[] distance = new int[N+1];
    Arrays.fill(distance, -1);
    //新建已访问节点的集合
    boolean[] visited = new boolean[N+1];

    //将K加入到已访问集合
    visited[K] = true;
    distance[K] = 0;

    //从邻接表中读取 K 的邻接节点 并将邻接节点加入到可访问集合中
    for (int[] ints : adjGraph.get(K)) {
        distance[ints[0]] = ints[1];
    }

    //开始 dijkstra算法
    while (true) {
        int index = 0;
        int minDis = Integer.MAX_VALUE;
        //遍历visited数组
        for (int i = 1; i <= N; i++) {
            if (visited[i] || distance[i] == -1)
                continue;
            if (distance[i] < minDis) {
                minDis = distance[i];
                index = i;
            }
        }

        if (index == 0)
            break;
        //此时 确认新加入已访问集合的节点
        visited[index] = true;

        if (!adjGraph.containsKey(index))
            continue;

        //将该节点的邻接节点加入到可访问节点集合中 并且更新最短距离
        for (int[] ints : adjGraph.get(index)) {
            if (visited[ints[0]])
                continue;
            if (distance[ints[0]] == -1)
                distance[ints[0]] = minDis + ints[1];
            else if (minDis + ints[1] < distance[ints[0]])
                distance[ints[0]] = minDis + ints[1];
        }
    }

    int max = 0;
    for (int i = 1; i <= N; i++) {
        if (distance[i] == -1)
            return -1;
        max = Math.max(max, distance[i]);
    }

    return max;
}
```