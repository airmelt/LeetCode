# 3082 Find the Sum of the Power of All Subsequences 求出所有子序列的能量和

__Description:__

You are given an integer array `nums` of length `n` and a __positive__ integer `k`.

The __power__ of an array of integers is defined as the number of subsequences with their sum __equal__ to `k`.

Return _the __sum__ of __power__ of all subsequences of_ `nums`_._

Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3], k = 3

Output: 6

Explanation:
```

There are `5` subsequences of nums with non-zero power:

- The subsequence [__1__,__2__,__3__] has `2` subsequences with `sum == 3`: [1,2,3] and [1,2,3].
- The subsequence [__1__,2,__3__] has `1` subsequence with `sum == 3`: [1,2,3].
- The subsequence [1,__2__,__3__] has `1` subsequence with `sum == 3`: [1,2,3].
- The subsequence [__1__,__2__,3] has `1` subsequence with `sum == 3`: [1,2,3].
- The subsequence [1,2,__3__] has `1` subsequence with `sum == 3`: [1,2,3].

Hence the answer is `2 + 1 + 1 + 1 + 1 = 6`.

Example 2:

```text
Input: nums = [2,3,3], k = 5

Output: 4

Explanation:
```

There are `3` subsequences of nums with non-zero power:

- The subsequence [__2__,__3__,__3__] has 2 subsequences with `sum == 5`: [2,3,3] and [2,3,3].
- The subsequence [__2__,3,__3__] has 1 subsequence with `sum == 5`: [2,3,3].
- The subsequence [__2__,__3__,3] has 1 subsequence with `sum == 5`: [2,3,3].

Hence the answer is `2 + 1 + 1 = 4`.

Example 3:

```text
Input: nums = [1,2,3], k = 7
```

Output: 0

__Explanation:__ There exists no subsequence with sum `7`. Hence all subsequences of nums have `power = 0`.

__Constraints:__

- `1 <= n <= 100`
- `1 <= nums[i] <= 10 ^ 4`
- `1 <= k <= 100`

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums` 和一个 __正__ 整数 `k` 。

一个整数数组的 __能量__ 定义为和 __等于__ `k` 的子序列的数目。

请你返回 `nums` 中所有子序列的 __能量和__ 。

由于答案可能很大，请你将它对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入： nums = [1,2,3], k = 3

输出： 6

解释：
```

总共有 `5` 个能量不为 0 的子序列:

- 子序列 [___1___,___2___,___3___] 有 `2` 个和为 `3` 的子序列: [1,2,___3___] 和 [___1___,___2___,3] 。
- 子序列 [___1___,2,___3___] 有 `1` 个和为 `3` 的子序列: [1,2,___3___] 。
- 子序列 [1,___2___,___3___] 有 `1` 个和为 `3` 的子序列: [1,2,___3___] 。
- 子序列 [___1___,___2___,3] 有 `1` 个和为 `3` 的子序列: [___1___,___2___,3] 。
- 子序列 [1,2,___3___] 有 `1` 个和为 `3` 的子序列: [1,2,___3___] 。

所以答案为 `2 + 1 + 1 + 1 + 1 = 6` 。

示例 2：

```text
输入： nums = [2,3,3], k = 5

输出： 4

解释：
```

总共有 `3` 个能量不为 0 的子序列:

- 子序列 [___2___,___3___,___3___] 有 2 个子序列和为 `5` : [___2___,3,___3___] 和 [___2___,___3___,3] 。
- 子序列 [___2___,3,___3___] 有 1 个子序列和为 `5` : [___2___,3,___3___] 。
- 子序列 [___2___,___3___,3] 有 1 个子序列和为 `5` : [___2___,___3___,3] 。

所以答案为 `2 + 1 + 1 = 4` 。

示例 3：

```text
输入： nums = [1,2,3], k = 7

输出： 0
```

__解释:__ 不存在和为 `7` 的子序列，所以 `nums` 的能量和为 `0` 。

__提示：__

- `1 <= n <= 100`
- `1 <= nums[i] <= 10 ^ 4`
- `1 <= k <= 100`

__思路:__

```text
动态规划
设 dp[i][j] 表示 i + 1 个子序列凑成 k 一共能组成的子序列个数
初始化 dp[0][0] = 1, 表示 0 个子序列凑成 0 有 1 种方式
对于和等于 k 的子序列 s, 需要检查是否在一个更大的子序列 t 中
所以对于 nums[i]
要么不在 s 也不在 t 中 dp[i + 1][j] = dp[i][j]
要么不在 s 中但是在 t 中 dp[i + 1][j] = dp[i][j]
要么在 s 中且在 t 中 dp[i + 1][j] = dp[i][j - nums[i]]
上面 3 种情况求和 dp[i + 1][j] = 2 * dp[i][j] + dp[i][j - nums[j]]
j 必须满足不小于 nums[i]
由于 dp[i + 1] 只和 dp[i] 有关
可以使用滚动数组降低时间复杂度
时间复杂度为 O(NK), 空间复杂度为 O(K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumOfPower(vector<int>& nums, int k) 
    {
        vector<long long> dp(k + 1);
        dp.front() = 1LL;
        long long MOD = 1e9 + 7LL;
        for (const auto& num : nums) for (int j = k; j > -1; j--) dp[j] = ((dp[j] << 1LL) + (j >= num ? dp[j - num] : 0LL)) % MOD;
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int sumOfPower(int[] nums, int k) {
        long dp[] = new long[k + 1], MOD = 1_000_000_007L;
        dp[0] = 1L;
        for (int num : nums) for (int j = k; j > -1; j--) dp[j] = ((dp[j] << 1L) + (j >= num ? dp[j - num] : 0L)) % MOD;
        return (int)dp[k];
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfPower(self, nums: List[int], k: int) -> int:
        dp, MOD = [1] + [0] * k, 10 ** 9 + 7
        for num in nums:
            for j in range(k, -1, -1):
                dp[j] = ((dp[j - num] if j >= num else 0) + (dp[j] << 1)) % MOD
        return dp[k]
```
