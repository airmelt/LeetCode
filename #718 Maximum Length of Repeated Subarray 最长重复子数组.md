
# 718 Maximum Length of Repeated Subarray 最长重复子数组

__Description__:
Given two integer arrays nums1 and nums2, return the maximum length of a subarray that appears in both arrays.

__Example:__

Example 1:

Input: nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
Output: 3
Explanation: The repeated subarray with maximum length is [3,2,1].

Example 2:

Input: nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
Output: 5

__Constraints:__

1 <= nums1.length, nums2.length <= 1000
0 <= nums1[i], nums2[i] <= 100

__题目描述__:
给两个整数数组 A 和 B ，返回两个数组中公共的、长度最长的子数组的长度。

__示例 :__

输入：
A: [1,2,3,2,1]
B: [3,2,1,4,7]
输出：3
解释：
长度最长的公共子数组是 [3, 2, 1] 。

__提示:__

1 <= len(A), len(B) <= 1000
0 <= A[i], B[i] < 100

__思路__:

1. 动态规划
设 dp[i + 1][j + 1] 表示以 nums1[i] 和 nums2[j] 结尾的子数组的长度
若 nums1[i] == nums2[j], dp[i + 1][j + 1] = dp[i][j] + 1
否则 dp[i + 1][j + 1] = 0
返回 dp 数组中的最大值
时间复杂度为 O(mn), 空间复杂度为 O(mn)
2. 滑动窗口
固定 nums1 数组, 将 nums2 数组从 nums1[0] 滑动到 nums1[m - 1], 记录下滑动窗口的最大值
固定 nums2 数组, 将 nums1 数组从 nums2[0] 滑动到 nums2[n - 1], 记录下滑动窗口的最大值
返回滑动窗口的最大值
时间复杂度为 O(m ^ 2 \* n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) 
    {
        int m = nums1.size(), n = nums2.size(), result = 0;
        for (int diff = -(m - 1); diff <= n - 1; diff++) for (int i = max(0, -diff), l = (nums1[i] == nums2[i + diff]) ? 1 : 0; i < min(m, n - diff); i++, l = i < min(m, n - diff) ? ((nums1[i] == nums2[i + diff]) ? (l + 1) : 0) : 0) result = max(result, l);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length, result = 0;
        for (int diff = -(m - 1); diff <= n - 1; diff++) for (int i = Math.max(0, -diff), l = (nums1[i] == nums2[i + diff]) ? 1 : 0; i < Math.min(m, n - diff); i++, l = i < Math.min(m, n - diff) ? ((nums1[i] == nums2[i + diff]) ? (l + 1) : 0) : 0) result = Math.max(result, l);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        m, n = len(nums1), len(nums2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(m):
            for j in range(n):
                dp[i + 1][j + 1] = dp[i][j] + 1 if nums1[i] == nums2[j] else 0
        return max(max(row) for row in dp)
```
