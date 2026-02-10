# 2897 Apply Operations on Array to Maximize Sum of Squares 对数组执行操作使平方和最大

__Description:__

You are given a __0-indexed__ integer array `nums` and a __positive__ integer `k`.

You can do the following operation on the array any number of times:

- Choose any two distinct indices `i` and `j` and __simultaneously__ update the values of `nums[i]` to `(nums[i] AND nums[j])` and `nums[j]` to `(nums[i] OR nums[j])`. Here, `OR` denotes the bitwise `OR` operation, and `AND` denotes the bitwise `AND` operation.

You have to choose `k` elements from the final array and calculate the sum of their __squares__.

Return the maximum sum of squares you can achieve.

Since the answer can be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [2,6,5,8], k = 2
Output: 261
Explanation: We can do the following operations on the array:
```

- Choose i = 0 and j = 3, then change nums[0] to (2 AND 8) = 0 and nums[3] to (2 OR 8) = 10. The resulting array is nums = [0,6,5,10].
- Choose i = 2 and j = 3, then change nums[2] to (5 AND 10) = 0 and nums[3] to (5 OR 10) = 15. The resulting array is nums = [0,6,0,15].

We can choose the elements 15 and 6 from the final array. The sum of squares is 152 + 62 = 261.
It can be shown that this is the maximum value we can get.

Example 2:

```text
Input: nums = [4,5,4,7], k = 3
Output: 90
Explanation: We do not need to apply any operations.
We can choose the elements 7, 5, and 4 with a sum of squares: 72 + 52 + 42 = 90.
It can be shown that this is the maximum value we can get.
```

__Constraints:__

- `1 <= k <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个 __正__ 整数 `k` 。

你可以对数组执行以下操作 任意次 ：

- 选择两个互不相同的下标 `i` 和 `j` ，__同时__ 将 `nums[i]` 更新为 `(nums[i] AND nums[j])` 且将 `nums[j]` 更新为 `(nums[i] OR nums[j])` ， `OR` 表示按位 __或__ 运算， `AND` 表示按位 __与__ 运算。

你需要从最终的数组里选择 `k` 个元素，并计算它们的 __平方__ 之和。

请你返回你可以得到的 最大 平方和。

由于答案可能会很大，将答案对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入：nums = [2,6,5,8], k = 2
输出：261
解释：我们可以对数组执行以下操作：
```

- 选择 i = 0 和 j = 3 ，同时将 nums[0] 变为 (2 AND 8) = 0 且 nums[3] 变为 (2 OR 8) = 10 ，结果数组为 nums = [0,6,5,10] 。
- 选择 i = 2 和 j = 3 ，同时将 nums[2] 变为 (5 AND 10) = 0 且 nums[3] 变为 (5 OR 10) = 15 ，结果数组为 nums = [0,6,0,15] 。

从最终数组里选择元素 15 和 6 ，平方和为 152 + 62 = 261 。
261 是可以得到的最大结果。

示例 2：

```text
输入：nums = [4,5,4,7], k = 3
输出：90
解释：不需要执行任何操作。
选择元素 7 ，5 和 4 ，平方和为 72 + 52 + 42 = 90 。
90 是可以得到的最大结果。
```

__提示：__

- `1 <= k <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
位运算
可以先猜测将 1 集中起来会使得平方和最大
对于 a 和 b, 同增同减一个相同的数 d, a > b
(a + d) ^ 2 + (b - d) ^ 2 = a ^ 2 + 2ad + d ^ 2 + b ^ 2 - 2bd + d ^ 2 = a ^ 2 + b ^ 2 + 2d ^ 2 + 2d(a - b) > 0
故 1 集中起来会使得平方和最大
统计 1 的个数
将 1 尽可能分配到一个数上
统计平方和并求模即可
时间复杂度为 O(N), 空间复杂度为 O(1), 实际上还需要 O(logC) 的时空复杂度, 在题目范围 C 不超过 10 ^ 9, 即 logC 最大为 30
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxSum(vector<int>& nums, int k) 
    {
        int c[MAX]{ 0 };
        long long result = 0LL, MOD = 1e9 + 7LL;
        for (const auto& i : nums) for (int j = 0; j < MAX; j++) if (i >> j & 1) ++c[j];
        while (k--) 
        {
            long long cur = 0LL;
            for (int j = 0; j < MAX; j++) if (c[j]-- > 0) cur |= 1LL << j;
            result = (result + cur * cur) % MOD;
        }
        return result;
    }
private:
    static constexpr int MAX = 30;
};
```

__Java__:

```Java
class Solution {
    public int maxSum(List<Integer> nums, int k) {
        int MAX = 30, c[] = new int[MAX];
        long result = 0L, MOD = 1_000_000_007L;
        for (int i : nums) for (int j = 0; j < MAX; j++) if ((i >> j & 1) == 1) ++c[j];
        while (k-- > 0) {
            long cur = 0L;
            for (int j = 0; j < MAX; j++) if (c[j]-- > 0) cur |= 1L << j;
            result = (result + cur * cur) % MOD;
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSum(self, nums: List[int], k: int) -> int:
        c, result = [0] * (MAX := 30), 0
        for i in nums:
            for j in range(MAX):
                c[j] += i >> j & 1
        for _ in range(k):
            cur = 0
            for j in range(MAX):
                if c[j]:
                    cur |= (1 << j)
                    c[j] -= 1
            result += cur ** 2
        return result % (10 ** 9 + 7)
```
