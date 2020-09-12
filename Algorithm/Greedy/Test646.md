# 最长数对链
给出 n 个数对。 在每一个数对中，第一个数字总是比第二个数字小。

现在，我们定义一种跟随关系，当且仅当 b < c 时，数对(c, d) 才可以跟在 (a, b) 后面。我们用这种形式来构造一个数对链。

给定一个对数集合，找出能够形成的最长数对链的长度。你不需要用到所有的数对，你可以以任何顺序选择其中的一些数对来构造。

**示例 :**
```java
输入: [[1,2], [2,3], [3,4]]
输出: 2
解释: 最长的数对链是 [1,2] -> [3,4]
```
## 解题思路 贪心
此题目可以看作是安排行程，在一定的时间段内安排尽量多的行程。按照贪心的思想，我们要使每段行程的截止日期尽可能的早，所以我们选择对截至日期进行排序。

### java代码
```java
class Solution {
    public int findLongestChain(int[][] pairs) {
        Arrays.sort(pairs, (a,b) -> {
            return a[1] - b[1];
        });
        int temp = pairs[0][1];
        int count = 1;
        for (int i = 1; i < pairs.length; i++) {
            if (pairs[i][0] > temp) {
                count++;
                temp = pairs[i][1];
            }
        }
        return count;
    }
}
```