# 1799 Maximize Score After N Operations N 次操作后的最大分数和

__Description:__

You are given `nums`, an array of positive integers of size `2 * n`. You must perform `n` operations on this array.

In the `i ^ th` operation __(1-indexed)__, you will:

- Choose two elements, `x` and `y`.
- Receive a score of `i * gcd(x, y)`.
- Remove `x` and `y` from `nums`.

Return _the maximum score you can receive after performing_ `n` _operations._

The function `gcd(x, y)` is the greatest common divisor of `x` and `y`.

__Example:__

Example 1:

```text
Input: nums = [1,2]
Output: 1
Explanation: The optimal choice of operations is:
(1 * gcd(1, 2)) = 1
```

Example 2:

```text
Input: nums = [3,4,6,8]
Output: 11
Explanation: The optimal choice of operations is:
(1 * gcd(3, 6)) + (2 * gcd(4, 8)) = 3 + 8 = 11
```

Example 3:

```text
Input: nums = [1,2,3,4,5,6]
Output: 14
Explanation: The optimal choice of operations is:
(1 * gcd(1, 5)) + (2 * gcd(2, 4)) + (3 * gcd(3, 6)) = 1 + 4 + 9 = 14
```

__Constraints:__

- `1 <= n <= 7`
- `nums.length == 2 * n`
- `1 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你 `nums` ，它是一个大小为 `2 * n` 的正整数数组。你必须对这个数组执行 `n` 次操作。

在第 `i` 次操作时（操作编号从 __1__ 开始），你需要:

- 选择两个元素 `x` 和 `y` 。
- 获得分数 `i * gcd(x, y)` 。
- 将 `x` 和 `y` 从 `nums` 中删除。

请你返回 `n` 次操作后你能获得的分数和最大为多少。

函数 `gcd(x, y)` 是 `x` 和 `y` 的最大公约数。

__示例:__

示例 1：

```text
输入：nums = [1,2]
输出：1
解释：最优操作是：
(1 * gcd(1, 2)) = 1
```

示例 2：

```text
输入：nums = [3,4,6,8]
输出：11
解释：最优操作是：
(1 * gcd(3, 6)) + (2 * gcd(4, 8)) = 3 + 8 = 11
```

示例 3：

```text
输入：nums = [1,2,3,4,5,6]
输出：14
解释：最优操作是：
(1 * gcd(1, 5)) + (2 * gcd(2, 4)) + (3 * gcd(3, 6)) = 1 + 4 + 9 = 14
```

__提示：__

- `1 <= n <= 7`
- `nums.length == 2 * n`
- `1 <= nums[i] <= 10 ^ 6`

__思路:__

```text
状态压缩 ➕ 动态规划
n 的值只有 7, 可以使用状态压缩
dp[mask] 表示状态为 mask 时的最大分数
其中 mask 的二进制为 1 的时候表示对应 nums 下标还未删去
状态转移方程为: dp[mask] = max(dp[mask ^ (1 << i) ^ (1 << j)] + (k >> 1) * gcd(nums[i], nums[j]), dp[mask]), 其中 k 为 mask 的二进制 1 的个数, 即 k 表示删除的轮次
每次需要删除 2 个数, 所以 k 为奇数的时候不合法
检查 mask 删除数的时候, 可以使用移位和与操作检查是否合法
最后返回 dp[m - 1] 即可
时间复杂度为 O(2 ^ N * N ^ 2), 空间复杂度为 O(2 ^ N), N 为 nums 的长度, 实际上 N 不会超过 14
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxScore(vector<int>& nums) 
    {
        int n = nums.size(), m = 1 << n;
        vector<int> dp(m);
        for (int mask = 2, k; mask < m; mask++)
        {
            if ((k = __builtin_popcount(mask)) & 1) continue;
            for (int i = 0; i < n; i++)
            {
                if (!((1 << i) & mask)) continue;
                for (int j = i + 1; j < n; j++) 
                {
                    if (!((1 << j) & mask)) continue;
                    dp[mask] = max(dp[mask], dp[mask ^ (1 << i) ^ (1 << j)] + (k >> 1) * __gcd(nums[i], nums[j]));
                }
            }
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maxScore(int[] nums) {
        int n = nums.length, m = 1 << n, dp[] = new int[m];
        for (int mask = 2, k; mask < m; mask++) {
            if (((k = Integer.bitCount(mask)) & 1) != 0) continue;
            for (int i = 0; i < n; i++) {
                if (((1 << i) & mask) == 0) continue;
                for (int j = i + 1; j < n; j++) {
                    if (((1 << j) & mask) == 0) continue;
                    dp[mask] = Math.max(dp[mask], dp[mask ^ (1 << i) ^ (1 << j)] + (k >> 1) * gcd(nums[i], nums[j]));
                }
            }
        }
        return dp[m - 1];
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def maxScore(self, nums: List[int]) -> int:
        dp = [0] * (m := (1 << (n := len(nums))))
        for mask in range(2, m):
            if (k := bin(mask).count('1')) & 1:
                continue
            for i in range(n):
                if not ((1 << i) & mask):
                    continue
                for j in range(i + 1, n):
                    if not ((1 << j) & mask):
                        continue
                    dp[mask] = max(dp[mask], dp[mask ^ (1 << i) ^ (1 << j)] + (k >> 1) * gcd(nums[i], nums[j]))
        return dp[-1]
```
