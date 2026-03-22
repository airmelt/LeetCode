# 1848 Minimum Distance to the Target Element 到目标元素的最小距离

__Description:__

Given an integer array `nums` __(0-indexed)__ and two integers `target` and `start`, find an index `i` such that `nums[i] == target` and `abs(i - start)` is __minimized__. Note that `abs(x)` is the absolute value of `x`.

Return `abs(i - start)`.

It is __guaranteed__ that `target` exists in `nums`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4,5], target = 5, start = 3
Output: 1
Explanation: nums[4] = 5 is the only value equal to target, so the answer is abs(4 - 3) = 1.
```

Example 2:

```text
Input: nums = [1], target = 1, start = 0
Output: 0
Explanation: nums[0] = 1 is the only value equal to target, so the answer is abs(0 - 0) = 0.
```

Example 3:

```text
Input: nums = [1,1,1,1,1,1,1,1,1,1], target = 1, start = 0
Output: 0
Explanation: Every value of nums is 1, but nums[0] minimizes abs(i - start), which is abs(0 - 0) = 0.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 4`
- `0 <= start < nums.length`
- `target` is in `nums`.

__题目描述:__

给你一个整数数组 `nums` （下标 __从 0 开始__ 计数）以及两个整数 `target` 和 `start` ，请你找出一个下标 `i` ，满足 `nums[i] == target` 且 `abs(i - start)` __最小化__ 。注意: `abs(x)` 表示 `x` 的绝对值。

返回 `abs(i - start)` 。

题目数据保证 `target` 存在于 `nums` 中。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4,5], target = 5, start = 3
输出：1
解释：nums[4] = 5 是唯一一个等于 target 的值，所以答案是 abs(4 - 3) = 1 。
```

示例 2：

```text
输入：nums = [1], target = 1, start = 0
输出：0
解释：nums[0] = 1 是唯一一个等于 target 的值，所以答案是 abs(0 - 0) = 0 。
```

示例 3：

```text
输入：nums = [1,1,1,1,1,1,1,1,1,1], target = 1, start = 0
输出：0
解释：nums 中的每个值都是 1 ，但 nums[0] 使 abs(i - start) 的结果得以最小化，所以答案是 abs(0 - 0) = 0 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 4`
- `0 <= start < nums.length`
- `target` 存在于 `nums` 中

__思路:__

```text
模拟
初始化结果为数组长度
遍历数组找到与 target 相同的元素就更新结果
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getMinDistance(vector<int>& nums, int target, int start) 
    {
        int n = nums.size(), result = n;
        for (int i = 0; i < n; i++) if (nums[i] == target) if (abs(i - start) < result) result =  abs(i - start);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int getMinDistance(int[] nums, int target, int start) {
        int n = nums.length, result = n;
        for (int i = 0; i < n; i++) if (nums[i] == target) if (Math.abs(i - start) < result) result = Math.abs(i - start);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getMinDistance(self, nums: List[int], target: int, start: int) -> int:
        return min(abs(i - start) for i, num in enumerate(nums) if num == target)
```
