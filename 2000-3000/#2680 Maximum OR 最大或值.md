# 2680 Maximum OR 最大或值

__Description:__

You are given a __0-indexed__ integer array `nums` of length `n` and an integer `k`. In an operation, you can choose an element and multiply it by `2`.

Return _the maximum possible value of_ `nums[0] | nums[1] | ... | nums[n - 1]` _that can be obtained after applying the operation on nums at most_ `k` _times_.

Note that `a | b` denotes the __bitwise or__ between two integers `a` and `b`.

__Example:__

Example 1:

```text
Input: nums = [12,9], k = 1
Output: 30
Explanation: If we apply the operation to index 1, our new array nums will be equal to [12,18]. Thus, we return the bitwise or of 12 and 18, which is 30.
```

Example 2:

```text
Input: nums = [8,1,2], k = 2
Output: 35
Explanation: If we apply the operation twice on index 0, we yield a new array of [32,1,2]. Thus, we return 32|1|2 = 35.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 15`

__题目描述:__

给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `nums` 和一个整数 `k` 。每一次操作中，你可以选择一个数并将它乘 `2` 。

你最多可以进行 `k` 次操作，请你返回 `nums[0] | nums[1] | ... | nums[n - 1]` 的最大值。

`a | b` 表示两个整数 `a` 和 `b` 的 __按位或__ 运算。

__示例:__

示例 1：

```text
输入：nums = [12,9], k = 1
输出：30
解释：如果我们对下标为 1 的元素进行操作，新的数组为 [12,18] 。此时得到最优答案为 12 和 18 的按位或运算的结果，也就是 30 。
```

示例 2：

```text
输入：nums = [8,1,2], k = 2
输出：35
解释：如果我们对下标 0 处的元素进行操作，得到新数组 [32,1,2] 。此时得到最优答案为 32|1|2 = 35 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 15`

__思路:__

```text
贪心 ➕ 前后缀
将 k 次都给一个数要比分散开给效果更好
所以只要判断将这 k 次给那一个数
即 pre | (x << k) | suf 最大
pre 为数组的前缀或和, 可以一边遍历一边获得
suf 为数组的后缀或和, suf[i - 1] = suf[i] | nums[i], suf[i] 表示 nums[i] | nums[i + 1] | ...
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumOr(vector<int>& nums, int k) 
    {
        int n = nums.size();
        vector<long long> suf(n);
        long long result = 0LL, pre = 0LL;
        for (int i = n - 1; i > 0; i--) suf[i - 1] = suf[i] | nums[i];
        for (int i = 0; i < n; i++) 
        {
            result = max(result, pre | ((long long)nums[i] << k) | suf[i]);
            pre |= nums[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumOr(int[] nums, int k) {
        int n = nums.length;
        long suf[] = new long[n], result = 0L, pre = 0L;
        for (int i = n - 1; i > 0; i--) suf[i - 1] = suf[i] | nums[i];
        for (int i = 0; i < n; i++) {
            result = Math.max(result, pre | ((long)nums[i] << k) | suf[i]);
            pre |= nums[i];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumOr(self, nums: List[int], k: int) -> int:
        suf = [0] * ((n := len(nums)) + 1)
        for i in range(n - 1, 0, -1):
            suf[i - 1] = suf[i] | nums[i]
        result = pre = 0
        for i, x in enumerate(nums):
            result = max(result, pre | (x << k) | suf[i])
            pre |= x
        return result
```
