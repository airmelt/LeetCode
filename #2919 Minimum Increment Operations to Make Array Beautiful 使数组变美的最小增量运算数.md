# 2919 Minimum Increment Operations to Make Array Beautiful 使数组变美的最小增量运算数

__Description:__

You are given a __0-indexed__ integer array `nums` having length `n`, and an integer `k`.

You can perform the following increment operation any number of times (including zero):

- Choose an index `i` in the range `[0, n - 1]`, and increase `nums[i]` by `1`.

An array is considered __beautiful__ if, for any __subarray__ with a size of `3` or __more__, its __maximum__ element is __greater than or equal__ to `k`.

Return _an integer denoting the __minimum__ number of increment operations needed to make_ `nums` ___beautiful__._

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [2,3,0,0,2], k = 4
Output: 3
Explanation: We can perform the following increment operations to make nums beautiful:
Choose index i = 1 and increase nums[1] by 1 -> [2,4,0,0,2].
Choose index i = 4 and increase nums[4] by 1 -> [2,4,0,0,3].
Choose index i = 4 and increase nums[4] by 1 -> [2,4,0,0,4].
The subarrays with a size of 3 or more are: [2,4,0], [4,0,0], [0,0,4], [2,4,0,0], [4,0,0,4], [2,4,0,0,4].
In all the subarrays, the maximum element is equal to k = 4, so nums is now beautiful.
It can be shown that nums cannot be made beautiful with fewer than 3 increment operations.
Hence, the answer is 3.
```

Example 2:

```text
Input: nums = [0,1,3,3], k = 5
Output: 2
Explanation: We can perform the following increment operations to make nums beautiful:
Choose index i = 2 and increase nums[2] by 1 -> [0,1,4,3].
Choose index i = 2 and increase nums[2] by 1 -> [0,1,5,3].
The subarrays with a size of 3 or more are: [0,1,5], [1,5,3], [0,1,5,3].
In all the subarrays, the maximum element is equal to k = 5, so nums is now beautiful.
It can be shown that nums cannot be made beautiful with fewer than 2 increment operations.
Hence, the answer is 2.
```

Example 3:

```text
Input: nums = [1,1,2], k = 1
Output: 0
Explanation: The only subarray with a size of 3 or more in this example is [1,1,2].
The maximum element, 2, is already greater than k = 1, so we don't need any increment operation.
Hence, the answer is 0.
```

__Constraints:__

- `3 <= n == nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`
- `0 <= k <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始、长度为 `n` 的整数数组 `nums` ，和一个整数 `k` 。

你可以执行下述 递增 运算 任意 次（可以是 0 次）：

- 从范围 `[0, n - 1]` 中选择一个下标 `i` ，并将 `nums[i]` 的值加 `1` 。

如果数组中任何长度 __大于或等于 3__ 的子数组，其 __最大__ 元素都大于或等于 `k` ，则认为数组是一个 __美丽数组__ 。

以整数形式返回使数组变为 美丽数组 需要执行的 最小 递增运算数。

子数组是数组中的一个连续 非空 元素序列。

__示例:__

示例 1：

```text
输入：nums = [2,3,0,0,2], k = 4
输出：3
解释：可以执行下述递增运算，使 nums 变为美丽数组：
选择下标 i = 1 ，并且将 nums[1] 的值加 1 -> [2,4,0,0,2] 。
选择下标 i = 4 ，并且将 nums[4] 的值加 1 -> [2,4,0,0,3] 。
选择下标 i = 4 ，并且将 nums[4] 的值加 1 -> [2,4,0,0,4] 。
长度大于或等于 3 的子数组为 [2,4,0], [4,0,0], [0,0,4], [2,4,0,0], [4,0,0,4], [2,4,0,0,4] 。
在所有子数组中，最大元素都等于 k = 4 ，所以 nums 现在是美丽数组。
可以证明无法用少于 3 次递增运算使 nums 变为美丽数组。
因此，答案为 3 。
```

示例 2：

```text
输入：nums = [0,1,3,3], k = 5
输出：2
解释：可以执行下述递增运算，使 nums 变为美丽数组：
选择下标 i = 2 ，并且将 nums[2] 的值加 1 -> [0,1,4,3] 。
选择下标 i = 2 ，并且将 nums[2] 的值加 1 -> [0,1,5,3] 。
长度大于或等于 3 的子数组为 [0,1,5]、[1,5,3]、[0,1,5,3] 。
在所有子数组中，最大元素都等于 k = 5 ，所以 nums 现在是美丽数组。
可以证明无法用少于 2 次递增运算使 nums 变为美丽数组。 
因此，答案为 2 。
```

示例 3：

```text
输入：nums = [1,1,2], k = 1
输出：0
解释：在这个示例中，只有一个长度大于或等于 3 的子数组 [1,1,2] 。
其最大元素 2 已经大于 k = 1 ，所以无需执行任何增量运算。
因此，答案为 0 。
```

__提示：__

- `3 <= n == nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`
- `0 <= k <= 10 ^ 9`

__思路:__

```text
动态规划
显然只需要考虑本身及前 3 个数的情况
另外由于只需要大于等于 k
所以大于 k 的都当作是 k
a, b, c 分别表示当前数需要累加的值, 前一个数以及前前一个数需要累加的值
那么
a = min(a, b, c) + max(k - num, 0)
b = a
c = b
同时更新上述三个结果
即现在的数需要加上自己需要累加的量以及前 3 个数中的最小值
前一个数和前前一个数依次更新为之前的结果即可
最后返回 min(a, b, c) 表示最后三个数取最小值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minIncrementOperations(vector<int>& nums, int k) 
    {
        long long a = 0LL, b = 0LL, c = 0LL;
        for (const auto& num : nums) 
        {
            long long cur = min({a, b, c});
            c = b;
            b = a;
            a = cur + max(k - num, 0);
        }
        return min({a, b, c});
    }
};
```

__Java__:

```Java
class Solution {
    public long minIncrementOperations(int[] nums, int k) {
        long a = 0L, b = 0L, c = 0L;
        for (int num : nums) {
            long cur = Math.min(Math.min(a, b), c);
            c = b;
            b = a;
            a = cur + Math.max(k - num, 0);
        }
        return Math.min(Math.min(a, b), c);
    }
}
```

__Python__:

```Python
class Solution:
    def minIncrementOperations(self, nums: List[int], k: int) -> int:
        a = b = c = 0
        for num in nums:
            a, b, c = max(k - num, 0) + min(a, b, c), a, b
        return min(a, b, c)
```
