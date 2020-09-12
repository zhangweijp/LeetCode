# 俄罗斯套娃信封问题
给定一些标记了宽度和高度的信封，宽度和高度以整数对形式 (w, h) 出现。当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。

请计算最多能有多少个信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。

**说明:**
不允许旋转信封。

**示例:**
```
输入: envelopes = [[5,4],[6,4],[6,7],[2,3]]
输出: 3 
解释: 最多信封的个数为 3, 组合为: [2,3] => [5,4] => [6,7]。
```
## 解题思路
### 最长递增子序列（贪心）
最长递增子序列，已知信封有长和宽两个属性， 我们对其中一个属性(长度)进行从小到大的排序，在对另一个属性(宽度)进行逆向排序，这时我们已经确定长度时递增的了，我们在对宽度进行最长递增子序列的查找，这样找出来的的序列就一定是，长度和宽度都递增的序列。
### java代码
```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        Arrays.sort(envelopes,(a,b) -> {
            if (a[0] == b[0])
                return b[1] - a[1];
            else
                return a[0] - b[0];
        });
    }

    public int LIS(int[][] evlps) {
        int[] minEnd = new int[evlps.length];
        int len =1;
        minEnd[0] = evlps[0][1];
        for (int i = 1; i < midEnd.length; i++) {
            if (evlps[i][1] > minEnd[len-1]) {
                minEnd[len] = evlps[i][1];
                len++;
            } else {
                binarySearch(midEnd, 0, len-1, nums[i]);
            }
        }
        return len;
    }

    public void binarySearch(int[] nums, int left, int right, int target) {
        int mid = (right - left)/2 + left;
        while (left <= right) {
            if (nums[mid] < target) 
                right = mid + 1;
            else if (nums[mid] > target)
                left = mid - 1;
            else
                return;
            mid = (right - left)/2 + left;
        }
        nums[left] = target;
    }
}
```