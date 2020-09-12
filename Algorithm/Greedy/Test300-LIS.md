# 最长上升子序列
给定一个无序的整数数组，找到其中最长上升子序列的长度。  
**实例：**
```
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```
说明:
- 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
- 你算法的时间复杂度应该为 O(n2)。

## 解题方法
### 贪心
考虑一个简单的贪心，如果我们要是上升子序列尽可能的长，那么我们就要尽可能的使当前的最长子序列的结尾元素尽可能的小。

基于这样的贪心思路，我们维护一个数组 minEnd ，midEnd[i]就表示长度为i的最长上升子序列的末尾元素的最小值，用len记录当前最长子序列的长度。初始时len = 1，minEnd[1] = nums[i]；我们依次遍历nums数组中的每个元素，如果nums[i] > minEnd[len - 1]，则将nums[i]添加到minEnd数组的末尾；反之如果nums[i] < minEnd[len - 1]则找到minEnd[k - 1] < nums[i] < minEnd[k],更新minEnd[k]的值。
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums.length == 0)
            return 0;
        int[] minEnd = new int[nums.length];
        int len = 1;
        minEnd[0] = nums[0];
        for (int i = 1; i < minEnd.length; i++) {
            //nums[i]比当前的最长子序列的末尾元素大 那我们就给这个序列添加一个新的末尾
            if (nums[i] > minEnd[len-1]) {
                minEnd[len] = nums[i];
                len++;
            } else 
                binarySearch(int[] nums, 0, len - 1, nums[i]);
        }
        return len;
    }
    //用target替换 minEnd[k - 1] < nums[i] < minEnd[k] 中的minEnd[k]
    public void binarySearch(int[] nums, int left, int right, int target) {
        int mid = (right - left)/2 + left;
        while (left <= right) {
            if (target > nums[mid]) {
                right = mid + 1;
            } else if (target < nums[mid]) {
                left = mid - 1;
            } else
                return;
            mid = (right - left)/2 + left;
        }
        nums[left] = target;
    }
}
```