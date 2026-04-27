# 1955 Count Number of Special Subsequences 统计特殊子序列的数目

__Description:__

A sequence is __special__ if it consists of a __positive__ number of `0`s, followed by a __positive__ number of `1`s, then a __positive__ number of `2`s.

- For example, `[0,1,2]` and `[0,0,1,1,1,2]` are special.
- In contrast, `[2,1,0]`, `[1]`, and `[0,1,2,0]` are not special.

Given an array `nums` (consisting of __only__ integers `0`, `1`, and `2`), return _the __number of different subsequences__ that are special_. Since the answer may be very large, __return it modulo__ `10 ^ 9 + 7`.

A subsequence of an array is a sequence that can be derived from the array by deleting some or no elements without changing the order of the remaining elements. Two subsequences are different if the set of indices chosen are different.

__Example:__

Example 1:

```text
Input: nums = [0,1,2,2]
Output: 3
```

Explanation: The special subsequences are bolded [__0__,__1__,__2__,2], [__0__,__1__,2,__2__], and [__0__,__1__,__2__,__2__].

Example 2:

```text
Input: nums = [2,2,0,0]
Output: 0
Explanation: There are no special subsequences in [2,2,0,0].
```

Example 3:

```text
Input: nums = [0,1,2,0,1,2]
Output: 7
Explanation: The special subsequences are bolded:
```

- [__0__,__1__,__2__,0,1,2]
- [__0__,__1__,2,0,1,__2__]
- [__0__,__1__,2,0,__1__,__2__]
- [__0__,__1__,__2__,0,1,__2__]
- [__0__,1,2,0,__1__,__2__]
- [0,1,2,__0__,__1__,__2__]
- [__0__,1,2,__0__,__1__,__2__]

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 2`

__题目描述:__

__特殊序列__ 是由 __正整数__ 个 `0` ，紧接着 __正整数__ 个 `1` ，最后 __正整数__ 个 `2` 组成的序列。

- 比方说， `[0,1,2]` 和 `[0,0,1,1,1,2]` 是特殊序列。
- 相反， `[2,1,0]` ， `[1]` 和 `[0,1,2,0]` 就不是特殊序列。

给你一个数组 `nums` （__仅__ 包含整数 `0`， `1` 和 `2`），请你返回 _不同特殊子序列的数目_ 。由于答案可能很大，请你将它对 `10 ^ 9 + 7` __取余__ 后返回。

一个数组的 子序列 是从原数组中删除零个或者若干个元素后，剩下元素不改变顺序得到的序列。如果两个子序列的 下标集合 不同，那么这两个子序列是 不同的 。

__示例:__

示例 1：

```text
输入：nums = [0,1,2,2]
输出：3
```

解释：特殊子序列为 [__0__,__1__,__2__,2], [__0__,__1__,2,__2__], 和 [__0__,__1__,__2__,__2__] 。

示例 2：

```text
输入：nums = [2,2,0,0]
输出：0
解释：数组 [2,2,0,0] 中没有特殊子序列。
```

示例 3：

```text
输入：nums = [0,1,2,0,1,2]
输出：7
解释：特殊子序列包括：
```

- [__0__,__1__,__2__,0,1,2]
- [__0__,__1__,2,0,1,__2__]
- [__0__,__1__,2,0,__1__,__2__]
- [__0__,__1__,__2__,0,1,__2__]
- [__0__,1,2,0,__1__,__2__]
- [0,1,2,__0__,__1__,__2__]
- [__0__,1,2,__0__,__1__,__2__]

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 2`

__思路:__

```text
动态规划
dp[i][0] 表示以 0 结尾的子序列的数目
dp[i][1] 表示以 0, 1 结尾的子序列的数目
dp[i][2] 表示以 0, 1, 2 结尾的子序列的数目, 即答案
状态转移方程:
nums[i] == 0 时, dp[i][0] = (dp[i - 1][0] << 1) + 1, 因为对当前的 0 可以选择加入或者不加入, 不加入的话就是 dp[i - 1][0], 加入的话就是 dp[i - 1][0] + 1
nums[i] == 1 时, dp[i][1] = dp[i - 1][0] + (dp[i - 1][1] << 1), 因为对当前的 1 可以选择加入或者不加入, 不加入的话就是 dp[i - 1][1], 加入的话就是 dp[i - 1][1] + dp[i - 1][0], 即可以接在 0 后面, 也可以接在 1 后面
nums[i] == 2 时, dp[i][2] = dp[i - 1][1] + (dp[i - 1][2] << 1), 因为对当前的 2 可以选择加入或者不加入, 不加入的话就是 dp[i - 1][2], 加入的话就是 dp[i - 1][2] + dp[i - 1][1], 即可以接在 1 后面, 也可以接在 2 后面
最后返回 dp[n - 1][2]
注意到 dp[i][j] 只与 dp[i - 1][j] 有关, 因此可以用滚动数组优化空间复杂度到 O(1)
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countSpecialSubsequences(vector<int>& nums) 
    {
        vector<long long> dp(3);
        for (const auto& num : nums) dp[num] = ((dp[num] << 1) + (!num ? 1 : dp[num - 1])) % (int)(1e9 + 7);
        return dp[2];
    }
};
```

__Java__:

```Java
class Solution {
    public int countSpecialSubsequences(int[] nums) {
        long MOD = 1_000_000_007, dp[] = new long[3];
        for (int num : nums) dp[num] = ((dp[num] << 1) + (num == 0 ? 1 : dp[num - 1])) % MOD;
        return (int)dp[2];
    }
}
```

__Python__:

```Python
class Solution:
    def countSpecialSubsequences(self, nums: List[int]) -> int:
        MOD, dp = 10 ** 9 + 7, [0] * 3
        for num in nums:
            dp[num] = (dp[num] << 1) + (1 if not num else dp[num - 1])
        return dp[2] % MOD
```
