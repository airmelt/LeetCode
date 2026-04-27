# 1035 Uncrossed Lines 不相交的线

__Description__:
You are given two integer arrays nums1 and nums2. We write the integers of nums1 and nums2 (in the order they are given) on two separate horizontal lines.

We may draw connecting lines: a straight line connecting two numbers nums1[i] and nums2[j] such that:

nums1[i] == nums2[j], and
the line we draw does not intersect any other connecting (non-horizontal) line.
Note that a connecting line cannot intersect even at the endpoints (i.e., each number can only belong to one connecting line).

Return the maximum number of connecting lines we can draw in this way.

__Example:__

Example 1:

![Uncrossed Lines](https://assets.leetcode.com/uploads/2019/04/26/142.png)

Input: nums1 = [1,4,2], nums2 = [1,2,4]
Output: 2
Explanation: We can draw 2 uncrossed lines as in the diagram.
We cannot draw 3 uncrossed lines, because the line from nums1[1] = 4 to nums2[2] = 4 will intersect the line from nums1[2]=2 to nums2[1]=2.

Example 2:

Input: nums1 = [2,5,1,2,5], nums2 = [10,5,2,1,5,2]
Output: 3

Example 3:

Input: nums1 = [1,3,7,1,7,5], nums2 = [1,9,2,5,1]
Output: 2

__Constraints:__

1 <= nums1.length, nums2.length <= 500
1 <= nums1[i], nums2[j] <= 2000

__题目描述__:
在两条独立的水平线上按给定的顺序写下 nums1 和 nums2 中的整数。

现在，可以绘制一些连接两个数字 nums1[i] 和 nums2[j] 的直线，这些直线需要同时满足满足：

 nums1[i] == nums2[j]
且绘制的直线不与任何其他连线（非水平线）相交。
请注意，连线即使在端点也不能相交：每个数字只能属于一条连线。

以这种方法绘制线条，并返回可以绘制的最大连线数。

__示例 :__

示例 1：

![不相交的线](https://assets.leetcode.com/uploads/2019/04/26/142.png)

输入：nums1 = [1,4,2], nums2 = [1,2,4]
输出：2
解释：可以画出两条不交叉的线，如上图所示。
但无法画出第三条不相交的直线，因为从 nums1[1]=4 到 nums2[2]=4 的直线将与从 nums1[2]=2 到 nums2[1]=2 的直线相交。

示例 2：

输入：nums1 = [2,5,1,2,5], nums2 = [10,5,2,1,5,2]
输出：3

示例 3：

输入：nums1 = [1,3,7,1,7,5], nums2 = [1,9,2,5,1]
输出：2

__提示:__

1 <= nums1.length, nums2.length <= 500
1 <= nums1[i], nums2[j] <= 2000

__思路__:

动态规划
本题实际上就是求两个数组的最长公共子序列
令 dp[i][j] 表示 nums1[:i] 和 nums2[:j] 中最长公共子序列的长度
if nums1[i] == nums2[j], dp[i][j] = dp[i - 1][j - 1] + 1
else, dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
注意到 dp 只与上一行的状态有关, 可以将空间复杂度优化 到 O(n)
时间复杂度为 O(mn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        vector<int> dp(n + 1);
        for (int i = 0; i < m; i++) {
            vector<int> cur(dp);
            for (int j = 0; j < n; j++) dp[j + 1] = nums1[i] == nums2[j] ? cur[j] + 1 : max(dp[j], cur[j + 1]);
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maxUncrossedLines(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length, dp[] = new int[n + 1];
        for (int i = 0; i < m; i++) {
            int[] cur = (int[])Arrays.copyOf(dp, n + 1);
            for (int j = 0; j < n; j++) dp[j + 1] = nums1[i] == nums2[j] ? cur[j] + 1 : Math.max(dp[j], cur[j + 1]);
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def maxUncrossedLines(self, nums1: List[int], nums2: List[int]) -> int:
        dp = [0] * ((n := len(nums2)) + 1)
        for i in range((m := len(nums1))):
            cur = dp[:]
            for j in range(n):
                dp[j + 1] = cur[j] + 1 if nums1[i] == nums2[j] else max(dp[j], cur[j + 1])
        return dp[-1]
```
