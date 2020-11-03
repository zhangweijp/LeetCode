# 冗余连接
在本问题中, 树指的是一个连通且无环的无向图。

输入一个图，该图由一个有着N个节点 (节点值不重复1, 2, ..., N) 的树及一条附加的边构成。附加的边的两个顶点包含在1到N中间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组。每一个边的元素是一对[u, v] ，满足 u < v，表示连接顶点u 和v的无向图的边。

返回一条可以删去的边，使得结果图是一个有着N个节点的树。如果有多个答案，则返回二维数组中最后出现的边。答案边 [u, v] 应满足相同的格式 u < v。

**示例 1：**
```
输入: [[1,2], [1,3], [2,3]]
输出: [2,3]
解释: 给定的无向图为:
  1
 / \
2 - 3
```
**示例 2：**
```
输入: [[1,2], [2,3], [3,4], [1,4], [1,5]]
输出: [1,4]
解释: 给定的无向图为:
5 - 1 - 2
    |   |
    4 - 3
```
## 解题思路
根据题意，我们知道本题中的这个无向图，是以n个节点的树为基础，增加一条边，使其成为一个拥有 n 条边，n 个节点的有环无向图。我们要知道的是，对于树来说，在任意两个节点之间，增加一条边，就多了一个环。那么，我们首先要找到图里的环，再把环中的最后一条边删除即可，那么我这里我们可以用到并查集来查找环。

## 并查集
我们遍历集合中的每条边，对于每条边的两个端点，找到它们所属的集合(根节点)，然后对其进行合并，若当前两个端点已经处于同一个集合，那么就可以说明图里面有环路，且该条边是环路的一部分。

### java代码
```java
public class FindRedundantConnection {
    int[] father;
    int[] res = new int[2];
    public int[] findRedundantConnection(int[][] edges) {
        //记录每个节点的根节点
        father = new int[edges.length + 1];

        //初始化根节点
        for (int i = 1; i < father.length; i++)
            father[i] = i;
        //根据边来进行合并
        for (int i = 0; i < edges.length; i++) {
            union(edges[i][0], edges[i][1]);
        }

        return res;
    }
    //合并函数
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            father[rootX] = rootY;
        } else {
            res[0] = x;
            res[1] = y;
        }
    }
    //查找函数
    public int find(int x) {
        while (x != father[x]) {
            father[x] = father[father[x]];
            x = father[x];
        }
        return x;
    }
}
```