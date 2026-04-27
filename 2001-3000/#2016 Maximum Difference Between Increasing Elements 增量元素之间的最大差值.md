# 2016 Maximum Difference Between Increasing Elements 增量元素之间的最大差值

__Description:__

Given a __0-indexed__ integer array `nums` of size `n`, find the __maximum difference__ between `nums[i]` and `nums[j]` (i.e., `nums[j] - nums[i]`), such that `0 <= i < j < n` and `nums[i] < nums[j]`.

Return _the __maximum difference__._ If no such `i` and `j` exists, return `-1`.

__Example:__

Example 1:

```text
Input: nums = [7,1,5,4]
Output: 4
Explanation:
The maximum difference occurs with i = 1 and j = 2, nums[j] - nums[i] = 5 - 1 = 4.
Note that with i = 1 and j = 0, the difference nums[j] - nums[i] = 7 - 1 = 6, but i > j, so it is not valid.
```

Example 2:

```text
Input: nums = [9,4,3,2]
Output: -1
Explanation:
There is no i and j such that i < j and nums[i] < nums[j].
```

Example 3:

```text
Input: nums = [1,5,2,10]
Output: 9
Explanation:
The maximum difference occurs with i = 0 and j = 3, nums[j] - nums[i] = 10 - 1 = 9.
```

__Constraints:__

- `n == nums.length`
- `2 <= n <= 1000`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，该数组的大小为 `n` ，请你计算 `nums[j] - nums[i]` 能求得的 __最大差值__ ，其中 `0 <= i < j < n` 且 `nums[i] < nums[j]` 。

返回 __最大差值__ 。如果不存在满足要求的 `i` 和 `j` ，返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [7,1,5,4]
输出：4
解释：
最大差值出现在 i = 1 且 j = 2 时，nums[j] - nums[i] = 5 - 1 = 4 。
注意，尽管 i = 1 且 j = 0 时 ，nums[j] - nums[i] = 7 - 1 = 6 > 4 ，但 i > j 不满足题面要求，所以 6 不是有效的答案。
```

示例 2：

```text
输入：nums = [9,4,3,2]
输出：-1
解释：
不存在同时满足 i < j 和 nums[i] < nums[j] 这两个条件的 i, j 组合。
```

示例 3：

```text
输入：nums = [1,5,2,10]
输出：9
解释：
最大差值出现在 i = 0 且 j = 3 时，nums[j] - nums[i] = 10 - 1 = 9 。
```

__提示：__

- `n == nums.length`
- `2 <= n <= 1000`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
1. 暴力法
按题目要求模拟即可
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 前缀和
记录 i 之前的最小值 pre, 遍历 i 之后的值，更新最大差值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumDifference(vector<int>& nums) 
    {
        int result = -1, pre = nums.front(), n = nums.size();
        for (int i = 0; i < n; pre = min(pre, nums[i++])) if (nums[i] > pre) result = max(result, nums[i] - pre);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumDifference(int[] nums) {
        int result = -1, n = nums.length;
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (nums[j] > nums[i]) result = Math.max(result, nums[j] - nums[i]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumDifference(self, nums: List[int]) -> int:
        return max([nums[j] - nums[i] for i in range(len(nums)) for j in range(i + 1, len(nums)) if nums[i] < nums[j]], default=-1)
```
