# 1793 Maximum Score of a Good Subarray 好子数组的最大分数

__Description:__

You are given an array of integers `nums` __(0-indexed)__ and an integer `k`.

The __score__ of a subarray `(i, j)` is defined as `min(nums[i], nums[i+1], ..., nums[j]) * (j - i + 1)`. A __good__ subarray is a subarray where `i <= k <= j`.

Return the maximum possible score of a good subarray.

__Example:__

Example 1:

```text
Input: nums = [1,4,3,7,4,5], k = 3
Output: 15
Explanation: The optimal subarray is (1, 5) with a score of min(4,3,7,4,5) * (5-1+1) = 3 * 5 = 15.
```

Example 2:

```text
Input: nums = [5,5,4,5,4,1,1,1], k = 0
Output: 20
Explanation: The optimal subarray is (0, 4) with a score of min(5,5,4,5,4) * (4-0+1) = 4 * 5 = 20.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 2 * 10 ^ 4`
- `0 <= k < nums.length`

__题目描述:__

给你一个整数数组 `nums` __（下标从 0 开始）__和一个整数 `k` 。

一个子数组 `(i, j)` 的 __分数__ 定义为 `min(nums[i], nums[i+1], ..., nums[j]) * (j - i + 1)` 。一个 __好__ 子数组的两个端点下标需要满足 `i <= k <= j` 。

请你返回 好 子数组的最大可能 分数 。

__示例:__

示例 1：

```text
输入：nums = [1,4,3,7,4,5], k = 3
输出：15
解释：最优子数组的左右端点下标是 (1, 5) ，分数为 min(4,3,7,4,5) * (5-1+1) = 3 * 5 = 15 。
```

示例 2：

```text
输入：nums = [5,5,4,5,4,1,1,1], k = 0
输出：20
解释：最优子数组的左右端点下标是 (0, 4) ，分数为 min(5,5,4,5,4) * (4-0+1) = 4 * 5 = 20 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 2 * 10 ^ 4`
- `0 <= k < nums.length`

__思路:__

```text
双指针
设置反向的双指针 left, right, 从 nums[k] 开始向两边扩展
从 val = nums[k] 开始向 0 扩展, 直到 val = 0
每次扩展时, 更新 left, right, 计算当前的分数
left 更新为左边第一个不小于 val 的位置
right 更新为右边第一个不小于 val 的位置
区间长度为 right - left + 1
区间高度为 val
更新结果为 max(result, 区间长度 * 区间高度)
时间复杂度为 O(N + M), 空间复杂度为 O(1), 其中 M 为 nums[k] 的值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumScore(vector<int>& nums, int k) 
    {
        int left = k, right = k, n = nums.size(), result = 0;
        for (int val = nums[k]; val > -1; --val) 
        {
            while (left > 0 and nums[left - 1] >= val) --left;
            while (right < n - 1 and nums[right + 1] >= val) ++right;
            result = max(result, val * (right - left + 1));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumScore(int[] nums, int k) {
        int left = k, right = k, n = nums.length, result = 0;
        for (int val = nums[k]; val > -1; --val) {
            while (left > 0 && nums[left - 1] >= val) --left;
            while (right < n - 1 && nums[right + 1] >= val) ++right;
            result = Math.max(result, val * (right - left + 1));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumScore(self, nums: List[int], k: int) -> int:
        left, right, n, result = k, k, len(nums), 0
        for val in range(nums[k], -1, -1):
            while left and nums[left - 1] >= val:
                left -= 1
            while right < n - 1 and nums[right + 1] >= val:
                right += 1
            result = max(result, val * (right - left + 1))
        return result
```
