# 1685 Sum of Absolute Differences in a Sorted Array 有序数组中差绝对值之和

__Description:__

You are given an integer array `nums` sorted in __non-decreasing__ order.

Build and return _an integer array_ `result` _with the same length as_ `nums` _such that_ `result[i]` _is equal to the __summation of absolute differences__ between_ `nums[i]` _and all the other elements in the array._

In other words, `result[i]` is equal to `sum(|nums[i]-nums[j]|)` where `0 <= j < nums.length` and `j != i` (__0-indexed__).

__Example:__

Example 1:

```text
Input: nums = [2,3,5]
Output: [4,3,5]
Explanation: Assuming the arrays are 0-indexed, then
result[0] = |2-2| + |2-3| + |2-5| = 0 + 1 + 3 = 4,
result[1] = |3-2| + |3-3| + |3-5| = 1 + 0 + 2 = 3,
result[2] = |5-2| + |5-3| + |5-5| = 3 + 2 + 0 = 5.
```

Example 2:

```text
Input: nums = [1,4,6,8,10]
Output: [24,15,13,15,21]
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= nums[i + 1] <= 10 ^ 4`

__题目描述:__

给你一个 __非递减__ 有序整数数组 `nums` 。

请你建立并返回一个整数数组 `result`，它跟 `nums` 长度相同，且 `result[i]` 等于 `nums[i]` 与数组中所有其他元素差的绝对值之和。

换句话说， `result[i]` 等于 `sum(|nums[i]-nums[j]|)` ，其中 `0 <= j < nums.length` 且 `j != i` （下标从 0 开始）。

__示例:__

示例 1：

```text
输入：nums = [2,3,5]
输出：[4,3,5]
解释：假设数组下标从 0 开始，那么
result[0] = |2-2| + |2-3| + |2-5| = 0 + 1 + 3 = 4，
result[1] = |3-2| + |3-3| + |3-5| = 1 + 0 + 2 = 3，
result[2] = |5-2| + |5-3| + |5-5| = 3 + 2 + 0 = 5。
```

示例 2：

```text
输入：nums = [1,4,6,8,10]
输出：[24,15,13,15,21]
```

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= nums[i + 1] <= 10 ^ 4`

__思路:__

```text
前缀和
由于数组已经有序
因此 |nums[i] - nums[j]| = nums[j] - nums[i] (i < j)
对于 nums[0], result[0] = sum(nums) - nums[0] * n
对于 nums[i], result[i] = result[i - 1] + (nums[i] - nums[i - 1]) * ((i << 1) - n)
也可以用前缀和
记 left[i] = sum(nums[0:i]), right[i] = sum(nums[i + 1:n])
则 right[i] - left[i] 表示所有 nums[j], i != j 的和
然后 nums[i] 自身的贡献为 nums[i] * (n - (i << 1) - 1)
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> getSumAbsoluteDifferences(vector<int>& nums) 
    {
        int n = nums.size();
        vector<int> left(n), right(n), result(n);
        for (int i = 1; i < n; i++) left[i] = left[i - 1] + nums[i - 1];
        for (int i = n - 2; i > -1; i--) right[i] = right[i + 1] + nums[i + 1];
        for (int i = 0; i < n; i++) result[i] = right[i] - left[i] - nums[i] * (n - (i << 1) - 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] getSumAbsoluteDifferences(int[] nums) {
        int n = nums.length, left[] = new int[n], right[] = new int[n], result[] = new int[n];
        for (int i = 1; i < n; i++) left[i] = left[i - 1] + nums[i - 1];
        for (int i = n - 2; i > -1; i--) right[i] = right[i + 1] + nums[i + 1];
        for (int i = 0; i < n; i++) result[i] = right[i] - left[i] - nums[i] * (n - (i << 1) - 1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getSumAbsoluteDifferences(self, nums: List[int]) -> List[int]:
        result = [sum(num - nums[0] for num in nums)] + [0] * ((n := len(nums)) - 1)
        for i in range(1, n):
            result[i] = result[i - 1] + (nums[i] - nums[i - 1]) * ((i << 1) - n)
        return result
```
