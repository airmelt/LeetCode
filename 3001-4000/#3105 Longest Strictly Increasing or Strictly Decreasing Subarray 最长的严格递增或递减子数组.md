# 3105 Longest Strictly Increasing or Strictly Decreasing Subarray 最长的严格递增或递减子数组

__Description:__

You are given an array of integers `nums`. Return _the length of the __longest__ subarray of_ `nums` _which is either __strictly increasing__ or __strictly decreasing__.

__Example:__

Example 1:

```text
Input: nums = [1,4,3,3,2]

Output: 2

Explanation:
```

The strictly increasing subarrays of `nums` are `[1]`, `[2]`, `[3]`, `[3]`, `[4]`, and `[1,4]`.

The strictly decreasing subarrays of `nums` are `[1]`, `[2]`, `[3]`, `[3]`, `[4]`, `[3,2]`, and `[4,3]`.

Hence, we return `2`.

Example 2:

```text
Input: nums = [3,3,3,3]

Output: 1

Explanation:
```

The strictly increasing subarrays of `nums` are `[3]`, `[3]`, `[3]`, and `[3]`.

The strictly decreasing subarrays of `nums` are `[3]`, `[3]`, `[3]`, and `[3]`.

Hence, we return `1`.

Example 3:

```text
Input: nums = [3,2,1]

Output: 3

Explanation:
```

The strictly increasing subarrays of `nums` are `[3]`, `[2]`, and `[1]`.

The strictly decreasing subarrays of `nums` are `[3]`, `[2]`, `[1]`, `[3,2]`, `[2,1]`, and `[3,2,1]`.

Hence, we return `3`.

__Constraints:__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 50`

__题目描述:__

给你一个整数数组 `nums` 。

返回数组 `nums` 中 __严格递增__ 或 __严格递减__ 的最长非空子数组的长度。

__示例:__

示例 1：

```text
输入：nums = [1,4,3,3,2]

输出：2

解释：
```

`nums` 中严格递增的子数组有 `[1]`、 `[2]`、 `[3]`、 `[3]`、 `[4]` 以及 `[1,4]` 。

`nums` 中严格递减的子数组有 `[1]`、 `[2]`、 `[3]`、 `[3]`、 `[4]`、 `[3,2]` 以及 `[4,3]` 。

因此，返回 `2` 。

示例 2：

```text
输入：nums = [3,3,3,3]

输出：1

解释：
```

`nums` 中严格递增的子数组有 `[3]`、 `[3]`、 `[3]` 以及 `[3]` 。

`nums` 中严格递减的子数组有 `[3]`、 `[3]`、 `[3]` 以及 `[3]` 。

因此，返回 `1` 。

示例 3：

```text
输入：nums = [3,2,1]

输出：3

解释：
```

`nums` 中严格递增的子数组有 `[3]`、 `[2]` 以及 `[1]` 。

`nums` 中严格递减的子数组有 `[3]`、 `[2]`、 `[1]`、 `[3,2]`、 `[2,1]` 以及 `[3,2,1]` 。

因此，返回 `3` 。

__提示：__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 50`

__思路:__

```text
分组循环
找到两个不相等的元素
先定下来是升序还是降序
然后如果满足一直一致就遍历直到数组末尾
更新结果
并从上一次的结尾的前一位开始重新遍历
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestMonotonicSubarray(vector<int>& nums) 
    {
        int result = 1, i = 0, n = nums.size();
        while (i < n - 1) 
        {
            if (nums[i + 1] == nums[i]) 
            {
                ++i;
                continue;
            }
            int start = i, inc = nums[i + 1] > nums[i];
            i += 2;
            while (i < n and nums[i] != nums[i - 1] and (nums[i] > nums[i - 1]) == inc) ++i;
            result = max(result, i-- - start);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestMonotonicSubarray(int[] nums) {
        int result = 1, i = 0, n = nums.length;
        while (i < n - 1) {
            if (nums[i + 1] == nums[i]) {
                ++i;
                continue;
            }
            int start = i;
            boolean inc = nums[i + 1] > nums[i];
            i += 2;
            while (i < n && nums[i] != nums[i - 1] && (nums[i] > nums[i - 1]) == inc) ++i;
            result = Math.max(result, i-- - start);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestMonotonicSubarray(self, nums: List[int]) -> int:
        return max(max([j - i for i in range(len(nums)) for j in range(i + 1, len(nums) + 1) if all(nums[k] > nums[k - 1] for k in range(i + 1, j)) or all(nums[k] < nums[k - 1] for k in range(i + 1, j))], default=-1), 1)
```
