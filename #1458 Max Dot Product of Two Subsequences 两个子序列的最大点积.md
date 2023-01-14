# 1458 Max Dot Product of Two Subsequences 两个子序列的最大点积

__Description:__

Given two arrays  `nums1` and  `nums2`.

Return the maximum dot product between non-empty subsequences of nums1 and nums2 with the same length.

A subsequence of a array is a new array which is formed from the original array by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie,  `[2,3,5]` is a subsequence of  `[1,2,3,4,5]` while  `[1,5,3]` is not).

__Example:__

Example 1:

```text
Input: nums1 = [2,1,-2,5], nums2 = [3,0,-6]
Output: 18
Explanation: Take subsequence [2,-2] from nums1 and subsequence [3,-6] from nums2.
Their dot product is (2*3 + (-2)*(-6)) = 18.
```

Example 2:

```text
Input: nums1 = [3,-2], nums2 = [2,-6,7]
Output: 21
Explanation: Take subsequence [3] from nums1 and subsequence [7] from nums2.
Their dot product is (3*7) = 21.
```

Example 3:

```text
Input: nums1 = [-1,-1], nums2 = [1,1]
Output: -1
Explanation: Take subsequence [-1] from nums1 and subsequence [1] from nums2.
Their dot product is -1.
```

__Constraints:__

- `1 <= nums1.length, nums2.length <= 500`
- `-1000 <= nums1[i], nums2[i] <= 1000`

__题目描述:__

给你两个数组  `nums1` 和  `nums2` 。

请你返回  `nums1` 和  `nums2` 中两个长度相同的 __非空__ 子序列的最大点积。

数组的非空子序列是通过删除原数组中某些元素（可能一个也不删除）后剩余数字组成的序列，但不能改变数字间相对顺序。比方说， `[2,3,5]` 是  `[1,2,3,4,5]` 的一个子序列而  `[1,5,3]` 不是。

__示例:__

示例 1：

```text
输入：nums1 = [2,1,-2,5], nums2 = [3,0,-6]
输出：18
解释：从 nums1 中得到子序列 [2,-2] ，从 nums2 中得到子序列 [3,-6] 。
它们的点积为 (2*3 + (-2)*(-6)) = 18 。
```

示例 2：

```text
输入：nums1 = [3,-2], nums2 = [2,-6,7]
输出：21
解释：从 nums1 中得到子序列 [3] ，从 nums2 中得到子序列 [7] 。
它们的点积为 (3*7) = 21 。
```

示例 3：

```text
输入：nums1 = [-1,-1], nums2 = [1,1]
输出：-1
解释：从 nums1 中得到子序列 [-1] ，从 nums2 中得到子序列 [1] 。
它们的点积为 -1 。
```

__提示：__

- `1 <= nums1.length, nums2.length <= 500`
- `-1000 <= nums1[i], nums2[i] <= 100`

点积：

定义  __`a`__ = `[a1, a2,…, an]` 和 __`b`__ = `[b1, b2,…, bn]` 的点积为：

![1458-1](https://pic.leetcode-cn.com/1666164309-PBJMQp-image.png)

这里的 __Σ__ 指示总和符号。

![1458-2](https://pic.leetcode-cn.com/1666164309-PBJMQp-image.png)

__思路:__

```text
动态规划
设 dp[i][j] 表示 nums1[:i] 和 nums2[:j] 的点积的最大值
如果不选择 nums1[i - 1] 和 nums2[j - 1], dp[i][j] = dp[i - 1][j - 1]
如果选择 nums1[i - 1], 不选择 nums2[j - 1], dp[i][j] = dp[i - 1][j]
如果选择 nums2[j - 1], 不选择 nums1[i - 1], dp[i][j] = dp[i][j - 1]
如果选择 nums1[i - 1], nums2[j - 1], dp[i][j] = dp[i - 1][j - 1] + nums1[i - 1] * nums2[j - 1]
dp[i][j] 取决于以上 4 种情况的最大值
初始化 dp 为 -0x3f3f3f3f, 即 -inf
由于 dp[i] 只取决于 dp[i - 1], 可以用滚动数组优化空间复杂度 
时间复杂度为 O(MN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    int maxDotProduct(vector<int>& nums1, vector<int>& nums2) 
    {
        int m = nums1.size(), n = nums2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, -0x3f3f3f3f));
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) dp[i][j] = max({dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1] + nums1[i - 1] * nums2[j - 1], nums1[i - 1] * nums2[j - 1]});
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maxDotProduct(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length, inf = 0x3f3f3f3f, dp[][] = new int[m + 1][n + 1];
        for (int i = 0; i <= m; i++) Arrays.fill(dp[i], -inf);
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) dp[i][j] = Math.max(Math.max(nums1[i - 1] * nums2[j - 1], dp[i - 1][j - 1] + nums1[i - 1] * nums2[j - 1]), Math.max(dp[i - 1][j], dp[i][j - 1]));
        return dp[m][n];
    }
}
```

__Python__:

```Python
class Solution:
    def maxDotProduct(self, nums1: List[int], nums2: List[int]) -> int:
        m, dp = len(nums1), [-0x3f3f3f3f] * ((n := len(nums2)) + 1)
        for i in range(1, m + 1):
            cur = [-0x3f3f3f3f] * (n + 1)
            for j in range(1, n + 1):
                cur[j] = max(nums1[i - 1] * nums2[j - 1] + (dp[j - 1] >= 0) * dp[j - 1], dp[j - 1], dp[j], cur[j - 1])
            dp = cur
        return dp[-1]
```
