# 1658 Minimum Operations to Reduce X to Zero 将x减到0的最小操作数

__Description:__

You are given an integer array `nums` and an integer `x`. In one operation, you can either remove the leftmost or the rightmost element from the array `nums` and subtract its value from `x`. Note that this __modifies__ the array for future operations.

Return _the __minimum number__ of operations to reduce_ `x` _to __exactly___ `0` _if it is possible__, otherwise, return_ `-1`.

__Example:__

Example 1:

```text
Input: nums = [1,1,4,2,3], x = 5
Output: 2
Explanation: The optimal solution is to remove the last two elements to reduce x to zero.
```

Example 2:

```text
Input: nums = [5,6,7,8,9], x = 4
Output: -1
```

Example 3:

```text
Input: nums = [3,2,20,1,1,3], x = 10
Output: 5
Explanation: The optimal solution is to remove the last three elements and the first two elements (5 operations in total) to reduce x to zero.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 4`
- `1 <= x <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `x` 。每一次操作时，你应当移除数组 `nums` 最左边或最右边的元素，然后从 `x` 中减去该元素的值。请注意，需要 __修改__ 数组以供接下来的操作使用。

如果可以将 `x` __恰好__ 减到 `0` ，返回 __最小操作数__ ；否则，返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [1,1,4,2,3], x = 5
输出：2
解释：最佳解决方案是移除后两个元素，将 x 减到 0 。
```

示例 2：

```text
输入：nums = [5,6,7,8,9], x = 4
输出：-1
```

示例 3：

```text
输入：nums = [3,2,20,1,1,3], x = 10
输出：5
解释：最佳解决方案是移除后三个元素和前两个元素（总共 5 次操作），将 x 减到 0 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 4`
- `1 <= x <= 10 ^ 9`

__思路:__

```text
滑动窗口
从正面思考比较难
从反面思考，即从数组中找到最长的连续子数组，使得其和为 sum(nums) - x
如果存在这样的子数组，那么剩下的元素和为 x
只需要找到最长的窗口使得其和为 sum(nums) - x 即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums, int x) 
    {
        int result = -1, left = 0, right = 0, cur = 0, target = accumulate(nums.begin(), nums.end(), 0) - x, n = nums.size();
        while (right < n) 
        {
            cur += nums[right++];
            while (cur > target and left < n) cur -= nums[left++];
            if (cur == target) result = max(result, right - left);
        }
        return result == -1 ? result : n - result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums, int x) {
        int result = -1, left = 0, right = 0, cur = 0, target = 0, n = nums.length;
        for (int num: nums) target += num;
        target -= x;
        while (right < n) {
            cur += nums[right++];
            while (cur > target && left < n) cur -= nums[left++];
            if (cur == target) result = Math.max(result, right - left);
        }
        return result == -1 ? result : n - result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int], x: int) -> int:
        result, left, right, target, n, cur = -1, 0, 0, sum(nums) - x, len(nums), 0
        while right < n:
            cur += nums[right]
            while cur > target and left < n:
                cur -= nums[left]
                left += 1
            if cur == target:
                result = max(result, right - left + 1)
            right += 1
        return result if result == -1 else n - result
```
