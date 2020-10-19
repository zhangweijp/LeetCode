# 火柴拼正方形
还记得童话《卖火柴的小女孩》吗？现在，你知道小女孩有多少根火柴，请找出一种能使用所有火柴拼成一个正方形的方法。不能折断火柴，可以把火柴连接起来，并且每根火柴都要用到。

输入为小女孩拥有火柴的数目，每根火柴用其长度表示。输出即为是否能用所有的火柴拼成正方形。

**示例 1:**
```
输入: [1,1,2,2,2]
输出: true

解释: 能拼成一个边长为2的正方形，每边两根火柴。
```
**示例 2:**
```
输入: [3,3,3,3,4]
输出: false

解释: 不能用所有火柴拼成一个正方形。
```
## 解题思路
根据已知条件“不能折断火柴，可以把火柴连接起来，并且每根火柴都要用到”，我们可以对数组进行初步筛选，
- **每根火柴都要用到**  
  遍历数组求出所有火柴的长度之和 sum，如果 sum 不能被4整除，那么就一定不能拼成正方形；
- **不能折断火柴**  
  根据 sum 求出正方形的边长 len，再次遍历数组，如果出现任意一根火柴的长度大于 len，那么也一定不能拼成正方形；

题目中问的是火柴能否拼成一个正方形，其实就是问这个数组能否恰好切割成四个和相等的子集，看上去像是个背包问题，但是通过背包的思路似乎又可以解决（待议），题目又规定了数组的长度范围[0-15]，我又想到使用状态压缩和记忆化搜索的方法，用一个状态数组保存每个状态所对应的结果，但是逻辑上有些问题，只能解决部分样例
## DFS + 背包
DFS是什么思路？每根火柴有四个选择，遍历这四种选择，并且在做出选择之后，改变当前的状态并进入到下一根火柴的抉择过程，知道搜索出符合条件的结果。  
我们用一个edge[4]数组来分别保存四个子集需要的火柴长度，然后进行深度遍历即可
### java代码及其注解
```java
    class Solution {
    int n;
    public boolean makesquare(int[] nums) {
        this.n = nums.length;
        //不足四条边
        if(n < 4)
            return false;
        //排序可以优化剪枝
        Arrays.sort(nums);
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if (sum % 4 != 0)
            return false;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > sum/4)
                return false;
        }
        int[] sums = new int[4];
        Arrays.fill(sums, sum/4);
        return dfs(n-1, nums, sums);
    }

    //背包？ + dfs
    public boolean dfs(int index, int[] nums, int[] sums) {
        //遍历完整个数组 说明符合条件
        if (index == -1)
            return true;
        //每根火柴有四条边可以选择加入
        for (int i = 0; i < sums.length; i++) {
            //1. 当前火柴长度超过当前边所需要的长度
            //2. 当前边已经被填满
            if (nums[index] > sums[i] || sums[i] == 0)
                continue;
            sums[i] -= nums[index];
            if (dfs(index-1, nums, sums))
                return true;
            sums[i] += nums[index];
        }
        return false;
    }
}
```