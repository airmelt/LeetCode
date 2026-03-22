# 2741 Special Permutations 特别的排列

__Description:__

You are given a __0-indexed__ integer array `nums` containing `n` __distinct__ positive integers. A permutation of `nums` is called special if:

- For all indexes `0 <= i < n - 1`, either `nums[i] % nums[i+1] == 0` or `nums[i+1] % nums[i] == 0`.

Return _the total number of special permutations._ As the answer could be large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [2,3,6]
Output: 2
Explanation: [3,6,2] and [2,6,3] are the two special permutations of nums.
```

Example 2:

```text
Input: nums = [1,4,3]
Output: 2
Explanation: [3,1,4] and [4,1,3] are the two special permutations of nums.
```

__Constraints:__

- `2 <= nums.length <= 14`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，它包含 `n` 个 __互不相同__ 的正整数。如果 `nums` 的一个排列满足以下条件，我们称它是一个特别的排列:

- 对于 `0 <= i < n - 1` 的下标 `i` ，要么 `nums[i] % nums[i+1] == 0` ，要么 `nums[i+1] % nums[i] == 0` 。

请你返回特别排列的总数目，由于答案可能很大，请将它对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入：nums = [2,3,6]
输出：2
解释：[3,6,2] 和 [2,6,3] 是 nums 两个特别的排列。
```

示例 2：

```text
输入：nums = [1,4,3]
输出：2
解释：[3,1,4] 和 [4,1,3] 是 nums 两个特别的排列。
```

__提示：__

- `2 <= nums.length <= 14`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
动态规划 ➕ 状态压缩
直接求全排列需要 O(N!) 会超时
设 dfs(cur, pre, used) 表示使用过的二进制对应的数为 used, pre 为上一个使用的数, cur 为当前遍历的数
dfs 下一次访问的数在 used 中需要没使用过, 可以用与运算判断
同时下一个数需要和之前的数的取模为 0, 也可以是之前的数和下一个数取模为 0
为了加速计算可以将取过的路径存在一个字典中
将这个过程转化为动态规划
dp[mask][i] 表示当前选择的集合为 mask(一个二进制数, 对应位为 1 表示可选) 时, 上一个数为 nums[i] 的取值
mask 的范围为 (0, 1 << n)
mask 一定不包含 i, 用 mask >> i & 1 为 0 来判断
可选集为 j, 则 mask >> j & 1 不为 0
检查 nums[i] 和 nums[j] 取模的值
如果成立则  dp[mask][i] += dp[mask ^ (1 << j)][j]
最后求出所有 dp[total ^ (1 << i)][i] 的和即为 total 的取值
记得对 MOD = 1_000_000_007 取余
时间复杂度为 O(N ^ 2 * 2 ^ N), 空间复杂度为 O(N * 2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int specialPerm(vector<int>& nums) 
    {
        int n = nums.size(), total = (1 << n) - 1;
        vector<vector<long long>> dp(total, vector<long long>(n));
        dp.front() = vector<long long>(n, 1LL);
        for (int mask = 1; mask < total; mask++) for (int i = 0; i < n; i++) if (!(mask >> i & 1)) for (int j = 0; j < n; j++) if ((mask >> j & 1) and (!(nums[i] % nums[j]) or !(nums[j] % nums[i]))) dp[mask][i] += dp[mask ^ (1 << j)][j];
        long long result = 0LL, MOD = 1e9 + 7LL;
        for (int i = 0; i < n; i++) result += dp[total ^ (1 << i)][i];
        return result % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int specialPerm(int[] nums) {
        int n = nums.length, total = (1 << n) - 1;
        long dp[][] = new long[total][n], result = 0L, MOD = 1_000_000_007L;
        Arrays.fill(dp[0], 1L);
        for (int mask = 1; mask < total; mask++) for (int i = 0; i < n; i++) if ((mask >> i & 1) == 0) for (int j = 0; j < n; j++) if ((mask >> j & 1) != 0 && (nums[i] % nums[j] == 0 || nums[j] % nums[i] == 0)) dp[mask][i] += dp[mask ^ (1 << j)][j];
        for (int i = 0; i < n; i++) result += dp[total ^ (1 << i)][i];
        return (int)(result % MOD);
    }
}
```

__Python__:

```Python
class Solution:
    def specialPerm(self, nums: List[int]) -> int:
        n, memo, MOD = len(nums), {}, 10 ** 9 + 7
        
        def dfs(cur: int, pre_num: int, used: int) -> int:
            if cur == n:
                return 1
            if (cur, pre_num, used) in memo:
                return memo[(cur, pre_num, used)]
            result = 0
            for i in range(n):
                if not (used >> i) & 1 and (not pre_num % nums[i] or not nums[i] % pre_num):
                    used |= (1 << i)
                    result += dfs(cur + 1, nums[i], used)
                    used &= ~(1 << i)
            memo[(cur, pre_num, used)] = result % MOD
            return result % MOD
        return dfs(0, 0, 0)
```
