# 2789 Largest Element in an Array after Merge Operations 合并后数组中的最大元素

__Description:__

You are given a __0-indexed__ array `nums` consisting of positive integers.

You can do the following operation on the array any number of times:

- Choose an index `i` such that `0 <= i < nums.length - 1` and `nums[i] <= nums[i + 1]`. Replace the element `nums[i + 1]` with `nums[i] + nums[i + 1]` and delete the element `nums[i]` from the array.

Return the value of the largest element that you can possibly obtain in the final array.

__Example:__

Example 1:

```text
Input: nums = [2,3,7,9,3]
Output: 21
Explanation: We can apply the following operations on the array:
```

- Choose i = 0. The resulting array will be nums = [5,7,9,3].
- Choose i = 1. The resulting array will be nums = [5,16,3].
- Choose i = 0. The resulting array will be nums = [21,3].

The largest element in the final array is 21. It can be shown that we cannot obtain a larger element.

Example 2:

```text
Input: nums = [5,3,3]
Output: 11
Explanation: We can do the following operations on the array:
```

- Choose i = 1. The resulting array will be nums = [5,6].
- Choose i = 0. The resulting array will be nums = [11].

There is only one element in the final array, which is 11.

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始、由正整数组成的数组 `nums` 。

你可以在数组上执行下述操作 任意 次：

- 选中一个同时满足 `0 <= i < nums.length - 1` 和 `nums[i] <= nums[i + 1]` 的下标 `i` 。将元素 `nums[i + 1]` 替换为 `nums[i] + nums[i + 1]` ，并从数组中删除元素 `nums[i]` 。

返回你可以从最终数组中获得的 最大 元素的值。

__示例:__

示例 1：

```text
输入：nums = [2,3,7,9,3]
输出：21
解释：我们可以在数组上执行下述操作：
```

- 选中 i = 0 ，得到数组 nums = [5,7,9,3] 。
- 选中 i = 1 ，得到数组 nums = [5,16,3] 。
- 选中 i = 0 ，得到数组 nums = [21,3] 。

最终数组中的最大元素是 21 。可以证明我们无法获得更大的元素。

示例 2：

```text
输入：nums = [5,3,3]
输出：11
解释：我们可以在数组上执行下述操作：
```

- 选中 i = 1 ，得到数组 nums = [5,6] 。
- 选中 i = 0 ，得到数组 nums = [11] 。

最终数组中只有一个元素，即 11 。

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__思路:__

```text
模拟
由于后面的数字会影响前面的数字
从后往前遍历
如果当前累计值比不小于下一个元素, 就累加
否则直接用下一个元素替换能得到更大值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    long long maxArrayValue(vector<int>& nums) {
        long long result = nums.back();
        for (int n = nums.size(), i = n - 2; i > -1; i--) result = (nums[i] <= result) ? (result + nums[i]) : nums[i];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maxArrayValue(int[] nums) {
        long result = nums[nums.length - 1];
        for (int n = nums.length, i = n - 2; i > -1; i--) result = (nums[i] <= result) ? (result + nums[i]) : nums[i];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxArrayValue(self, nums: List[int]) -> int:
        result, n = nums[-1], len(nums)
        for i in range(n - 2, -1, -1):
            result = result + nums[i] if nums[i] <= result else nums[i]
        return result
```
