# 冗余连接 II
在本问题中，有根树指满足以下条件的有向图。该树只有一个根节点，所有其他节点都是该根节点的后继。每一个节点只有一个父节点，除了根节点没有父节点。

输入一个有向图，该图由一个有着N个节点 (节点值不重复1, 2, ..., N) 的树及一条附加的边构成。附加的边的两个顶点包含在1到N中间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组。 每一个边 的元素是一对 [u, v]，用以表示有向图中连接顶点 u 和顶点 v 的边，其中 u 是 v 的一个父节点。

返回一条能删除的边，使得剩下的图是有N个节点的有根树。若有多个答案，返回最后出现在给定二维数组的答案。

**示例 1:**
```
输入: [[1,2], [1,3], [2,3]]
输出: [2,3]
解释: 给定的有向图如下:
  1
 / \
v   v
2-->3
```
**示例 2:**
```
输入: [[1,2], [2,3], [3,4], [4,1], [1,5]]
输出: [4,1]
解释: 给定的有向图如下:
5 <- 1 -> 2
     ^    |
     |    v
     4 <- 3
```

## 解题思路
本题中的树为有向图
根据树的特性 我们能知道，树的根节点入度为0，其他节点的入度均为1，此时我们向树中插入一条有向边
1. 若插入的边指向树的根节点，该图中会出现一个有向环，且所有节点的入度皆为 1；此时我们删除单向环中任意一条边，得到的都是一颗树，所以我们可以使用并查集找到单向环中的最后一条边。
2. 若插入的边指向树的非根节点，那么必然使得该节点的入度加一，也就是说图中必会出现一个入度为 2 的节点。
* 我们可以通过入度表找到该节点，和指向该节点的两条边，
* 我们按照顺序，选择第一条边加入到并查集中，
* 查询完毕后，如果所有的节点有共同的根节点，那就说明第二条边是多余的，反之，第一条便是多余的。

### java代码
```java
public class FindRedundantDirectedConnection {
    int[] res = new int[2];
    int[] parent;
    int[][] resChoice = new int[2][2];
    public int[] findRedundantDirectedConnection(int[][] edges) {
        //节点的数量
        int N = edges.length;
        int node = 0;
        // parent 记录每个节点的根节点
        // indeg 入度数组
        parent = new int[N+1];
        int[] indeg = new int[N+1];

        //对于每个节点 将自己初始化为根节点
        for (int i = 1; i < parent.length; i++) {
            parent[i] = i;
        }

        for (int[] edge : edges) {
            indeg[edge[1]]++;
            //找到入度为2的节点
            if (indeg[edge[1]] == 2) {
                //找到指向该节点的两条边
                resChoice[1] = edge;
                node = edge[1];
            } else {
                union(edge[0], edge[1]);
            }
        }

        if (node != 0) {
            for (int[] edge : edges) {
                if (edge[1] == node) {
                    resChoice[0] = edge;
                    break;
                }
            }

            int root = 0;

            for (int i = 1; i < parent.length; i++) {
                if (root == 0) {
                    root = find(i);
                    continue;
                }
                if (find(i) != root)
                    return resChoice[0];
            }
            return resChoice[1];
        }

        return res;
    }
    
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX == rootY) {
            res[0] = x;
            res[1] = y;
        } else {
            parent[rootY] = rootX;
        }
    }
    
    public int find(int x) {
        while (x != parent[x]) {
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }
}
```