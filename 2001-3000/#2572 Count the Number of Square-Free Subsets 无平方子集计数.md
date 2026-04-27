# 2572 Count the Number of Square-Free Subsets 无平方子集计数

__Description:__

You are given a positive integer __0-indexed__ array `nums`.

A subset of the array `nums` is __square-free__ if the product of its elements is a __square-free integer__.

A __square-free integer__ is an integer that is divisible by no square number other than `1`.

Return _the number of square-free non-empty subsets of the array_ __nums__. Since the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

A __non-empty__ __subset__ of `nums` is an array that can be obtained by deleting some (possibly none but not all) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

__Example:__

Example 1:

```text
Input: nums = [3,4,4,5]
Output: 3
Explanation: There are 3 square-free subsets in this example:
```

- The subset consisting of the 0th element [3]. The product of its elements is 3, which is a square-free integer.
- The subset consisting of the 3rd element [5]. The product of its elements is 5, which is a square-free integer.
- The subset consisting of 0th and 3rd elements [3,5]. The product of its elements is 15, which is a square-free integer.

It can be proven that there are no more than 3 square-free subsets in the given array.

Example 2:

```text
Input: nums = [1]
Output: 1
Explanation: There is 1 square-free subset in this example:
```

- The subset consisting of the 0th element [1]. The product of its elements is 1, which is a square-free integer.

It can be proven that there is no more than 1 square-free subset in the given array.

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 30`

__题目描述:__

给你一个正整数数组 `nums` 。

如果数组 `nums` 的子集中的元素乘积是一个 __无平方因子数__ ，则认为该子集是一个 __无平方__ 子集。

__无平方因子数__ 是无法被除 `1` 之外任何平方数整除的数字。

返回数组 `nums` 中 __无平方__ 且 __非空__ 的子集数目。因为答案可能很大，返回对 `10 ^ 9 + 7` 取余的结果。

`nums` 的 __非空子集__ 是可以由删除 `nums` 中一些元素（可以不删除，但不能全部删除）得到的一个数组。如果构成两个子集时选择删除的下标不同，则认为这两个子集不同。

__示例:__

```text
示例 1：

输入：nums = [3,4,4,5]
输出：3
解释：示例中有 3 个无平方子集：
```

- 由第 0 个元素 [3] 组成的子集。其元素的乘积是 3 ，这是一个无平方因子数。
- 由第 3 个元素 [5] 组成的子集。其元素的乘积是 5 ，这是一个无平方因子数。
- 由第 0 个和第 3 个元素 [3,5] 组成的子集。其元素的乘积是 15 ，这是一个无平方因子数。

可以证明给定数组中不存在超过 3 个无平方子集。

```text
示例 2：

输入：nums = [1]
输出：1
解释：示例中有 1 个无平方子集：
```

- 由第 0 个元素 [1] 组成的子集。其元素的乘积是 1 ，这是一个无平方因子数。

可以证明给定数组中不存在超过 1 个无平方子集。

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 30`

__思路:__

```text
动态规划
使用位运算记录每个数字的平方因子
使用一个数组 SF 记录每个数字的平方因子
SF[i] 的二进制表示中第 j 位为 1 表示 i 能被 2 ^ j 整除
SF[i] 的值为 -1 表示 i 能被某个平方质数整除
比如 SF[4] = -1, SF[6] = 0b11(2 * 3), SF[10] = 0b101(2 * 5)
设 dp[i] 表示能得到集合 i 的个数
对于每个数字 num, 如果 SF[num] > -1
则从大到小遍历所有能得到的集合 j
如果 j | SF[num] == j, 即 SF[num] 是 j 的子集
则可以将 num 加入到集合 j 中
dp[j] = dp[j] + dp[j ^ SF[num]], 分别是选择 SF[num] 和不选择 SF[num] 的情况
时间复杂度为 O(N2 ^ M), 空间复杂度为 O(2 ^ M), M 为 PRIMES 的长度, 即不大于 max(nums) 的质数的个数, 本题为 10
也可以通过补集计算
这时 dp[j ^ mask] = dp[j ^ mask] + dp[j] * cnt[num]
最后返回 dp 中所有非空集合的个数
时间复杂度为 O(N + Q2 ^ M), 空间复杂度为 O(2 ^ M), Q 为 SF 的长度
```

__代码:__

__C++__:

```C++
const int MOD = 1e9 + 7, M = 1 << 10;
int PRIMES[]{2, 3, 5, 7, 11, 13, 17, 19, 23, 29}, SF[31]{};

int init = []() 
{
    for (int i = 2; i < 32; i++)
    {
        for (int j = 0; j < 10; j++) 
        {
            if (!(i % PRIMES[j])) 
            {
                if (!(i % (PRIMES[j] * PRIMES[j]))) 
                {
                    SF[i] = -1;
                    break;
                }
                SF[i] |= 1 << j;
            }
        }
    }  
    return 0;
}();

class Solution 
{
public:
    int squareFreeSubsets(vector<int> &nums) 
    {
        int cnt[31]{}, pow2 = 1, mask = 0, c = 0;
        for (const auto& num : nums)
        {
            if (num == 1) pow2 = (pow2 << 1) % MOD;
            else ++cnt[num];
        }
        long dp[M]{pow2};
        for (int i = 2; i < 31; i++) 
        {
            if ((mask = SF[i]) > -1 and (c = cnt[i])) 
            {
                int other = (M - 1) ^ mask, j = other;
                do 
                { 
                    dp[j | mask] = (dp[j | mask] + dp[j] * c) % MOD;
                    j = (j - 1) & other;
                } 
                while (j != other);
            }
        }
        return accumulate(dp, dp + M, -1L) % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    private static final int[] PRIMES = new int[]{2, 3, 5, 7, 11, 13, 17, 19, 23, 29};
    private static final int MOD = 1_000_000_007, M = 1 << 10;
    private static final int[] SF = new int[31];

    static {
        for (int i = 2; i < 31; i++) {
            for (int j = 0; j < 10; j++) {
                if (i % PRIMES[j] == 0) {
                    if (i % (PRIMES[j] * PRIMES[j]) == 0) {
                        SF[i] = -1;
                        break;
                    }
                    SF[i] |= 1 << j;
                }
            }
        }
    }

    public int squareFreeSubsets(int[] nums) {
        int dp[] = new int[M], mask = 0;
        dp[0] = 1;
        for (int num : nums) if ((mask = SF[num]) > -1) for (int j = M - 1; j > mask - 1; j--) if ((j | mask) == j) dp[j] = (dp[j] + dp[j ^ mask]) % MOD;
        return ((int)(Arrays.stream(dp).mapToLong(a -> a).sum() % MOD) - 1 + MOD) % MOD;
    }
}
```

__Python__:

```Python
PRIMES, MOD, SF = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29], 10 ** 9 + 7, [0] * 31
for i in range(2, 31):
    for j, p in enumerate(PRIMES):
        if not i % p:
            if not i % (p ** 2):
                SF[i] = -1
                break
            SF[i] |= 1 << j
class Solution:
    def squareFreeSubsets(self, nums: List[int]) -> int:
        dp = [1] + [0] * ((m := (1 << len(PRIMES))) - 1)
        for num in nums:
            if (mask := SF[num]) > -1:
                for j in range(m - 1, mask - 1, -1):
                    if (j | mask) == j:
                        dp[j] = (dp[j] + dp[j ^ mask]) % MOD
        return (sum(dp) - 1) % MOD
```
