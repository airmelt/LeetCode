# 1994 The Number of Good Subsets 好子集的数目

__Description:__

You are given an integer array `nums`. We call a subset of `nums` __good__ if its product can be represented as a product of one or more __distinct prime__ numbers.

- For example, if `nums = [1, 2, 3, 4]`:

  - `[2, 3]`, `[1, 2, 3]`, and `[1, 3]` are __good__ subsets with products `6 = 2*3`, `6 = 2*3`, and `3 = 3` respectively.
  - `[1, 4]` and `[4]` are not __good__ subsets with products `4 = 2*2` and `4 = 2*2` respectively.

Return _the number of different __good__ subsets in_ `nums` ___modulo___ `10 ^ 9 + 7`.

A __subset__ of `nums` is any array that can be obtained by deleting some (possibly none or all) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4]
Output: 6
Explanation: The good subsets are:
```

- [1,2]: product is 2, which is the product of distinct prime 2.
- [1,2,3]: product is 6, which is the product of distinct primes 2 and 3.
- [1,3]: product is 3, which is the product of distinct prime 3.
- [2]: product is 2, which is the product of distinct prime 2.
- [2,3]: product is 6, which is the product of distinct primes 2 and 3.
- [3]: product is 3, which is the product of distinct prime 3.

Example 2:

```text
Input: nums = [4,2,3,15]
Output: 5
Explanation: The good subsets are:
```

- [2]: product is 2, which is the product of distinct prime 2.
- [2,3]: product is 6, which is the product of distinct primes 2 and 3.
- [2,15]: product is 30, which is the product of distinct primes 2, 3, and 5.
- [3]: product is 3, which is the product of distinct prime 3.
- [15]: product is 15, which is the product of distinct primes 3 and 5.

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 30`

__题目描述:__

给你一个整数数组 `nums` 。如果 `nums` 的一个子集中，所有元素的乘积可以表示为一个或多个 __互不相同的质数__ 的乘积，那么我们称它为 __好子集__ 。

- 比方说，如果 `nums = [1, 2, 3, 4]` :

  - `[2, 3]` ， `[1, 2, 3]` 和 `[1, 3]` 是 __好__ 子集，乘积分别为 `6 = 2*3` ， `6 = 2*3` 和 `3 = 3` 。
  - `[1, 4]` 和 `[4]` 不是 __好__ 子集，因为乘积分别为 `4 = 2*2` 和 `4 = 2*2` 。

请你返回 `nums` 中不同的 __好__ 子集的数目对 `10 ^ 9 + 7` __取余__ 的结果。

`nums` 中的 __子集__ 是通过删除 `nums` 中一些（可能一个都不删除，也可能全部都删除）元素后剩余元素组成的数组。如果两个子集删除的下标不同，那么它们被视为不同的子集。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4]
输出：6
解释：好子集为：
```

- [1,2]：乘积为 2 ，可以表示为质数 2 的乘积。
- [1,2,3]：乘积为 6 ，可以表示为互不相同的质数 2 和 3 的乘积。
- [1,3]：乘积为 3 ，可以表示为质数 3 的乘积。
- [2]：乘积为 2 ，可以表示为质数 2 的乘积。
- [2,3]：乘积为 6 ，可以表示为互不相同的质数 2 和 3 的乘积。
- [3]：乘积为 3 ，可以表示为质数 3 的乘积。

示例 2：

```text
输入：nums = [4,2,3,15]
输出：5
解释：好子集为：
```

- [2]：乘积为 2 ，可以表示为质数 2 的乘积。
- [2,3]：乘积为 6 ，可以表示为互不相同质数 2 和 3 的乘积。
- [2,15]：乘积为 30 ，可以表示为互不相同质数 2，3 和 5 的乘积。
- [3]：乘积为 3 ，可以表示为质数 3 的乘积。
- [15]：乘积为 15 ，可以表示为互不相同质数 3 和 5 的乘积。

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 30`

__思路:__

```text
状态压缩 ➕ 动态规划
好子集添加任意个 1 仍然是好子集
包括平方个质质因子的数字不能出现, 比如 4, 8, 9, ...
其他数字最多出现一次, 比如 2, 3, 5, 7, 10, ...
由于数字不超过 30, 可以预先枚举不大于 30 的质数, 一共 10 个
用一个 10 位的二进制数 mask 就能表示出现的质数
设 dp[mask] 表示当选择 mask 时可以组成的好子集的数目
dp[mask] = sum(dp[mask ^ subset] * freq[i]), 其中 subset 是 mask 的子集, freq[i] 是数字 i 出现的次数
dp[1] = 2 ^ freq[1], 因为 1 可以出现任意次
逆序遍历 mask, 保证子集先于 mask 被计算
时间复杂度为 O(N), 空间复杂度为 O(1), 只需要 2 ^ 10 = 1024 个状态
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfGoodSubsets(vector<int>& nums) 
    {
        vector<int> freq(NUM_MAX + 1), dp(TOTAL);
        for (const auto& num : nums) ++freq[num];
        dp.front() = 1;
        for (int i = 0; i < freq[1]; i++) dp.front() = (dp.front() << 1) % MOD;
        for (int i = 2; i <= NUM_MAX; i++) 
        {
            if (freq[i]) 
            {
                int cur = 0, flag = 1;
                for (int j = 0; j < N; j++) 
                {
                    if (!(i % (PRIMES[j] * PRIMES[j])))
                    {
                        flag = 0;
                        break;
                    }
                    if (!(i % PRIMES[j])) cur |= (1 << j);
                }
                if (flag) for (int mask = TOTAL - 1; mask > 0; mask--) if ((mask & cur) == cur) dp[mask] = (int)((dp[mask] + ((long)dp[mask ^ cur]) * freq[i]) % MOD);
            }
        }
        int result = 0;
        for (int mask = 1; mask < TOTAL; mask++) result = (result + dp[mask]) % MOD;
        return result;
    }
private:
    static constexpr int N = 10;
    static constexpr int TOTAL = 1 << N;
    static constexpr array<int, N> PRIMES = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};
    static constexpr int NUM_MAX = 30;
    static constexpr int MOD = 1e9 + 7;
};
```

__Java__:

```Java
class Solution {
    static final int[] PRIMES = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};
    static final int N = 10;
    static final int TOTAL = 1 << N;
    static final int NUM_MAX = 30;
    static final int MOD = 1_000_000_007;

    public int numberOfGoodSubsets(int[] nums) {
        int result = 0, freq[] = new int[NUM_MAX + 1], dp[] = new int[TOTAL];
        for (int num : nums) ++freq[num];
        dp[0] = 1;
        for (int i = 0; i < freq[1]; i++) dp[0] = (dp[0] << 1) % MOD;
        for (int i = 2; i <= NUM_MAX; i++) {
            if (freq[i] != 0) {
                int cur = 0, flag = 1;
                for (int j = 0; j < N; j++) {
                    if (i % (PRIMES[j] * PRIMES[j]) == 0) {
                        flag = 0;
                        break;
                    }
                    if (i % PRIMES[j] == 0) cur |= (1 << j);
                }
                if (flag == 1) for (int mask = TOTAL - 1; mask > 0; mask--) if ((mask & cur) == cur) dp[mask] = (int)((dp[mask] + ((long)dp[mask ^ cur]) * freq[i]) % MOD);
            }
        }
        for (int mask = 1; mask < TOTAL; mask++) result = (result + dp[mask]) % MOD;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfGoodSubsets(self, nums: List[int]) -> int:
        primes, MOD, freq, dp = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29], 10 ** 9 + 7, Counter(nums), [0] * (total := (1 << 10))
        dp[0] = pow(2, freq[1], MOD)
        for k, v in freq.items():
            if k != 1:
                cur, flag = 0, True
                for i, prime in enumerate(primes):
                    if not k % (prime ** 2):
                        flag = False
                        break
                    if not k % prime:
                        cur |= (1 << i)
                if flag:
                    for mask in range(total - 1, 0, -1):
                        if (mask & cur) == cur:
                            dp[mask] = (dp[mask] + dp[mask ^ cur] * v) % MOD
        return sum(dp[1:]) % MOD
```
