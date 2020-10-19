# 递增子序列
给定一个整型数组, 你的任务是找到所有该数组的递增子序列，递增子序列的长度至少是2。

**示例:**
```
输入: [4, 6, 7, 7]
输出: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]
```
**说明:**
1. 给定数组的长度不会超过15。
2. 数组中的整数范围是 [-100,100]。
3. 给定数组中可能包含重复数字，相等的数字应该被视为递增的一种情况。

## 解题思路
题目中使用"所有"、"全部"等意思的词语，这个时候我们就自然要联想到DFS、BFS等遍历算法进行枚举。
### DFS
从头开始遍历数组，对符合条件的元素来说，有两种选择，一是加入当前的递增子序列，二是不加入当前的递增子序列，而对于不符合条件的元素来说只有一种选择那就是不加入当前的递增子序列；数组中还有可能出现重复的元素，那么就有可能选择出重复的递增子序列，我们可以在代码中避免重复情况的出现。
### java代码
```java
public class FindSubsequences {
    int n;
    public List<List<Integer>> findSubsequences(int[] nums) {
        this.n = nums.length;
        List<List<Integer>> lists = new LinkedList<>();
        backtrack(nums, 0, -101, lists, new LinkedList<>());
        return lists;
    }

    public void backtrack(int[] nums, int cur, int lastNum, List<List<Integer>> lists, List<Integer> list) {
        //遍历到数组，且当前递增子序列的长度大于1时，将该序列添加到结果中
        if (cur == n) {
            if (list.size() > 1)
                lists.add(new LinkedList<>(list));
            return;
        }
        //当前元素可以添加到序列的尾部
        //  我们选择将其加入递增序列中
        //  我们将另一选择并入到下面的搜索中
        if (nums[cur] >= lastNum) {
            list.add(nums[cur]);
            backtrack(nums, cur + 1, nums[cur], lists, list);
            list.remove(list.size()-1);
        }
        //  我们不难发现 在当前元素和原序列末尾元素相等时，我们选择将其加入序列和选择不
        //  将其加入序列所调用的回溯方法是一样的，为了避免重复，我们减少一次调用
        if (nums[cur] != lastNum) {
            backtrack(nums, cur+1, lastNum, lists, list);
        }
    }

    public static void main(String[] args) {
        int[] nums = {4, 6, 7, 7};
        System.out.println(new FindSubsequences().findSubsequences(nums));
    }
}
```