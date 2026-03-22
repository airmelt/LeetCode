# 1655 Distribute Repeating Integers 分配重复整数

__Description:__

You are given an array of `n` integers, `nums`, where there are at most `50` unique values in the array. You are also given an array of `m` customer order quantities, `quantity`, where `quantity[i]` is the amount of integers the `i ^ th` customer ordered. Determine if it is possible to distribute `nums` such that:

- The `i ^ th` customer gets __exactly__ `quantity[i]` integers,
- The integers the `i ^ th` customer gets are __all equal__, and
- Every customer is satisfied.

Return `true` _if it is possible to distribute_ `nums` _according to the above conditions_.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4], quantity = [2]
Output: false
Explanation: The 0th customer cannot be given two different integers.
```

Example 2:

```text
Input: nums = [1,2,3,3], quantity = [2]
Output: true
Explanation: The 0th customer is given [3,3]. The integers [1,2] are not used.
```

Example 3:

```text
Input: nums = [1,1,2,2], quantity = [2,2]
Output: true
Explanation: The 0th customer is given [1,1], and the 1st customer is given [2,2].
```

__Constraints:__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 1000`
- `m == quantity.length`
- `1 <= m <= 10`
- `1 <= quantity[i] <= 10 ^ 5`
- There are at most `50` unique values in `nums`.

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums` ，这个数组中至多有 `50` 个不同的值。同时你有 `m` 个顾客的订单 `quantity` ，其中，整数 `quantity[i]` 是第 `i` 位顾客订单的数目。请你判断是否能将 `nums` 中的整数分配给这些顾客，且满足:

- 第 `i` 位顾客 __恰好__ 有 `quantity[i]` 个整数。
- 第 `i` 位顾客拿到的整数都是 __相同的__ 。
- 每位顾客都满足上述两个要求。

如果你可以分配 `nums` 中的整数满足上面的要求，那么请返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4], quantity = [2]
输出：false
解释：第 0 位顾客没办法得到两个相同的整数。
```

示例 2：

```text
输入：nums = [1,2,3,3], quantity = [2]
输出：true
解释：第 0 位顾客得到 [3,3] 。整数 [1,2] 都没有被使用。
```

示例 3：

```text
输入：nums = [1,1,2,2], quantity = [2,2]
输出：true
解释：第 0 位顾客得到 [1,1] ，第 1 位顾客得到 [2,2] 。
```

__提示：__

- `n == nums.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums[i] <= 1000`
- `m == quantity.length`
- `1 <= m <= 10`
- `1 <= quantity[i] <= 10 ^ 5`
- `nums` 中至多有 `50` 个不同的数字。

__思路:__

```text
子集枚举 ➕ 动态规划
先找到每个数字出现的频率, 先后次序不重要
因为 m 的最大为 10, 考虑用 m 做状态压缩枚举
设 dp[i][j] 表示 times 的前 i 个元素能否满足状态 j
如果 times[i] 满足 j 的某个子集 k, 并且 times[:i] 能够满足 j - k, 那么 dp[i - 1][j - k] 为 true, 且 k 的订单总和不超过 times[i]
这时 dp[i][j] 为 true
时间复杂度为 O(N * 3 ^ m), 空间复杂度为 O(N * 2 ^ m)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canDistribute(vector<int>& nums, vector<int>& quantity) 
    {
        unordered_map<int, int> um;
        for (const auto& x : nums) ++um[x];
        vector<int> times;
        for (const auto& [k, v] : um) times.emplace_back(v);
        int n = times.size(), m = quantity.size();
        vector<int> s(1 << m);
        for (int i = 1; i < (1 << m); i++) 
        {
            for (int j = 0; j < m; j++) 
            {
                if (i & (1 << j)) 
                {
                    s[i] = s[i - (1 << j)] + quantity[j];
                    break;
                }
            }
        }
        vector<vector<bool>> dp(n, vector<bool>(1 << m));
        for (int i = 0; i < n; i++) dp[i].front() = true;
        for (int i = 0; i < n; i++) 
        {
            for (int j = 1; j < (1 << m); j++) 
            {
                if (i and dp[i - 1][j]) 
                {
                    dp[i][j] = true;
                    continue;
                }
                for (int k = j; k; k = ((k - 1) & j)) 
                {
                    if ((!i ? !(j - k) : dp[i - 1][j - k]) and s[k] <= times[i])
                    {
                        dp[i][j] = true;
                        break;
                    }
                }
            }
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canDistribute(int[] nums, int[] quantity) {
        int[] count = new int[1001];
        for (int x : nums) ++count[x];
        List<Integer> times = new ArrayList<>();
        for (int v : count) if (v != 0) times.add(v);
        int n = times.size(), m = quantity.length, s[] = new int[1 << m];
        for (int i = 1; i < (1 << m); i++) {
            for (int j = 0; j < m; j++) {
                if ((i & (1 << j)) != 0) {
                    s[i] = s[i - (1 << j)] + quantity[j];
                    break;
                }
            }
        }
        boolean[][] dp = new boolean[n][1 << m];
        for (int i = 0; i < n; i++) dp[i][0] = true;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < (1 << m); j++) {
                if (i > 0 && dp[i - 1][j]) {
                    dp[i][j] = true;
                    continue;
                }
                for (int k = j; k != 0; k = ((k - 1) & j)) {
                    if ((i == 0 ? j - k == 0 : dp[i - 1][j - k]) && s[k] <= times.get(i)) {
                        dp[i][j] = true;
                        break;
                    }
                }
            }
        }
        return dp[n - 1][(1 << m) - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def canDistribute(self, nums: List[int], quantity: List[int]) -> bool:
        nums, n = list((c := Counter(nums)).values()), len(quantity)
        quantity.sort(reverse=True)
        
        @lru_cache(None)
        def trace_back(cur: int, use: Tuple[int]) -> bool:
            if cur == n:
                return True
            if use[-1] < quantity[cur]:
                return False
            use_list = list(use)
            for i in range(len(use_list) - 1, -1, -1):
                if use_list[i] >= quantity[cur]:
                    use_list[i] -= quantity[cur]
                    if trace_back(cur + 1, tuple(sorted(use_list))):
                        return True
                    use_list[i] += quantity[cur]
            return False
        return trace_back(0, tuple(sorted(nums)))
```
