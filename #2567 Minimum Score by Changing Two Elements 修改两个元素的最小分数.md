# 2567 Minimum Score by Changing Two Elements 修改两个元素的最小分数

__Description:__

You are given an integer array `nums`.

- The __low__ score of `nums` is the __minimum__ absolute difference between any two integers.
- The __high__ score of `nums` is the __maximum__ absolute difference between any two integers.
- The __score__ of `nums` is the sum of the __high__ and __low__ scores.

Return the __minimum score__ after __changing two elements__ of `nums`.

__Example:__

Example 1:

```text
Input: nums = [1,4,7,8,5]
Output: 3

Explanation:
```

- Change `nums[0]` and `nums[1]` to be 6 so that `nums` becomes [6,6,7,8,5].
- The low score is the minimum absolute difference: |6 - 6| = 0.
- The high score is the maximum absolute difference: |8 - 5| = 3.
- The sum of high and low score is 3.

Example 2:

```text
Input: nums = [1,4,3]
Output: 0

Explanation:
```

- Change `nums[1]` and `nums[2]` to 1 so that `nums` becomes [1,1,1].
- The sum of maximum absolute difference and minimum absolute difference is 0.

__Constraints:__

- `3 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。

- `nums` 的 __最小__ 得分是满足 `0 <= i < j < nums.length` 的 `|nums[i] - nums[j]|` 的最小值。
- `nums`的 __最大__ 得分是满足 `0 <= i < j < nums.length` 的 `|nums[i] - nums[j]|` 的最大值。
- `nums` 的分数是 __最大__ 得分与 __最小__ 得分的和。

我们的目标是最小化 `nums` 的分数。你 __最多__ 可以修改 `nums` 中 __2__ 个元素的值。

请你返回修改 `nums` 中 __至多两个__ 元素的值后，可以得到的 __最小分数__ 。

`|x|` 表示 `x` 的绝对值。

__示例:__

示例 1：

```text
输入: nums = [1,4,3]
输出: 0
解释: 将 nums[1] 和 nums[2] 的值改为 1 ，nums 变为 [1,1,1] 。 `|nums[i] - nums[j]|` 的值永远为 0 ，所以我们返回 0 + 0 = 0 。
```

示例 2：

```text
输入: nums = [1,4,7,8,5]
输出: 3
解释:
将 nums[0] 和 nums[1] 的值变为 6 ，nums 变为 [6,6,7,8,5] 。
最小得分是 i = 0 且 j = 1 时得到的 | `nums[i] - nums[j]`| = |6 - 6| = 0 。
最大得分是 i = 3 且 j = 4 时得到的 | `nums[i] - nums[j]`| = |8 - 5| = 3 。
最大得分与最小得分之和为 3 。这是最优答案。
```

__提示：__

- `3 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
排序
排序后, 最小分数的可能值为 nums[n - 3] - nums[0], nums[n - 2] - nums[1], nums[n - 1] - nums[0] 中的最小值
另外两个值修改为剩下的两个值即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimizeSum(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        return min({nums[nums.size() - 3] - nums[0], nums[nums.size() - 2] - nums[1], nums[nums.size() - 1] - nums[2]});
    }
};
```

__Java__:

```Java
class Solution {
    public int minimizeSum(int[] nums) {
        Arrays.sort(nums);
        return Math.min(Math.min(nums[nums.length - 3] - nums[0], nums[nums.length - 2] - nums[1]), nums[nums.length - 1] - nums[2]);
    }
}
```

__Python__:

```Python
class Solution:
    def minimizeSum(self, nums: List[int]) -> int:
        return 0 if not (nums := sorted(nums)) or (n := len(nums)) < 4 else min([nums[n + i - 3] - nums[i] for i in range(3)])
```
