# 2771 Longest Non-decreasing Subarray From Two Arrays 构造最长非递减子数组

__Description:__

You are given two __0-indexed__ integer arrays `nums1` and `nums2` of length `n`.

Let's define another __0-indexed__ integer array, `nums3`, of length `n`. For each index `i` in the range `[0, n - 1]`, you can assign either `nums1[i]` or `nums2[i]` to `nums3[i]`.

Your task is to maximize the length of the __longest non-decreasing subarray__ in `nums3` by choosing its values optimally.

Return _an integer representing the length of the __longest non-decreasing__ subarray in_ `nums3`.

Note: A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums1 = [2,3,1], nums2 = [1,2,1]
Output: 2
Explanation: One way to construct nums3 is: 
nums3 = [nums1[0], nums2[1], nums2[2]] => [2,2,1]. 
The subarray starting from index 0 and ending at index 1, [2,2], forms a non-decreasing subarray of length 2. 
We can show that 2 is the maximum achievable length.
```

Example 2:

```text
Input: nums1 = [1,3,2,1], nums2 = [2,2,3,4]
Output: 4
Explanation: One way to construct nums3 is: 
nums3 = [nums1[0], nums2[1], nums2[2], nums2[3]] => [1,2,3,4]. 
The entire array forms a non-decreasing subarray of length 4, making it the maximum achievable length.
```

Example 3:

```text
Input: nums1 = [1,1], nums2 = [2,2]
Output: 2
Explanation: One way to construct nums3 is: 
nums3 = [nums1[0], nums1[1]] => [1,1]. 
The entire array forms a non-decreasing subarray of length 2, making it the maximum achievable length.
```

__Constraints:__

- `1 <= nums1.length == nums2.length == n <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 10 ^ 9`

__题目描述:__

给你两个下标从 __0__ 开始的整数数组 `nums1` 和 `nums2` ，长度均为 `n` 。

让我们定义另一个下标从 __0__ 开始、长度为 `n` 的整数数组， `nums3` 。对于范围 `[0, n - 1]` 的每个下标 `i` ，你可以将 `nums1[i]` 或 `nums2[i]` 的值赋给 `nums3[i]` 。

你的任务是使用最优策略为 `nums3` 赋值，以最大化 `nums3` 中 __最长非递减子数组__ 的长度。

以整数形式表示并返回 `nums3` 中 __最长非递减__ 子数组的长度。

注意：子数组 是数组中的一个连续非空元素序列。

__示例:__

示例 1：

```text
输入：nums1 = [2,3,1], nums2 = [1,2,1]
输出：2
解释：构造 nums3 的方法之一是： 
nums3 = [nums1[0], nums2[1], nums2[2]] => [2,2,1]
从下标 0 开始到下标 1 结束，形成了一个长度为 2 的非递减子数组 [2,2] 。 
可以证明 2 是可达到的最大长度。
```

示例 2：

```text
输入：nums1 = [1,3,2,1], nums2 = [2,2,3,4]
输出：4
解释：构造 nums3 的方法之一是： 
nums3 = [nums1[0], nums2[1], nums2[2], nums2[3]] => [1,2,3,4]
整个数组形成了一个长度为 4 的非递减子数组，并且是可达到的最大长度。
```

示例 3：

```text
输入：nums1 = [1,1], nums2 = [2,2]
输出：2
解释：构造 nums3 的方法之一是： 
nums3 = [nums1[0], nums1[1]] => [1,1] 
整个数组形成了一个长度为 2 的非递减子数组，并且是可达到的最大长度。
```

__提示：__

- `1 <= nums1.length == nums2.length == n <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 10 ^ 9`

__思路:__

```text
动态规划
设 dp[i][0] 表示以 nums1[i] 结尾的最长非递减子数组的长度, dp[i][1] 表示以 nums2[i] 
则若 nums1[i] >= nums1[i - 1], 则可以从 dp[i - 1][0] 转移过来
dp[i][0] = max(dp[i][0], dp[i - 1][0] + 1)
其余情况类似
如果都不存在大于等于的情况那么初始化长度为 1, 即 dp[i][0] = dp[i][1] = 1, 表示以自身结尾的长度
最后返回 dp 数组里的最大值
由于 dp[i] 只和 dp[i - 1] 相关
可以使用滚动数组优化空间复杂度
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxNonDecreasingLength(vector<int>& nums1, vector<int>& nums2) 
    {
        int result = 1, f = 1, g = 1, n = nums1.size(), a = 0, b = 0;
        for (int i = 1; i < n; i++)
        {
            a = b = 1;
            if (nums1[i] >= nums1[i - 1]) a = f + 1;
            if (nums1[i] >= nums2[i - 1]) a = max(a, g + 1);
            if (nums2[i] >= nums1[i - 1]) b = f + 1;
            if (nums2[i] >= nums2[i - 1]) b = max(b, g + 1);
            result = max({ f = a, g = b, result });
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxNonDecreasingLength(int[] nums1, int[] nums2) {
        int n = nums1.length, dp[][] = new int[n][2], result = 1;
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], 1);
        for (int i = 1; i < n; i++) {
            if (nums1[i - 1] <= nums1[i]) dp[i][0] = Math.max(dp[i][0], dp[i - 1][0] + 1);
            if (nums2[i - 1] <= nums1[i]) dp[i][0] = Math.max(dp[i][0], dp[i - 1][1] + 1);
            if (nums1[i - 1] <= nums2[i]) dp[i][1] = Math.max(dp[i][1], dp[i - 1][0] + 1);
            if (nums2[i - 1] <= nums2[i]) dp[i][1] = Math.max(dp[i][1], dp[i - 1][1] + 1);
            result = Math.max(result, Math.max(dp[i][0], dp[i][1]));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxNonDecreasingLength(self, nums1: List[int], nums2: List[int]) -> int:
        result = f = g = 1
        for (x1, y1), (x2, y2) in pairwise(zip(nums1, nums2)):
            a = b = 1
            if x2 >= x1:
                a = f + 1
            if x2 >= y1:
                a = max(a, g + 1)
            if y2 >= x1:
                b = f + 1
            if y2 >= y1:
                b = max(b, g + 1)
            result = max(f := a, g := b, result)
        return result
```
