# 3117 Minimum Sum of Values by Dividing Array 划分数组得到最小的值之和

__Description:__

You are given two arrays `nums` and `andValues` of length `n` and `m` respectively.

The value of an array is equal to the last element of that array.

You have to divide `nums` into `m` __disjoint contiguous__ subarray such that for the `i ^ th` subarray `[li, ri]`, the bitwise `AND` of the subarray elements is equal to `andValues[i]`, in other words, `nums[li] & nums[li + 1] & ... & nums[ri] == andValues[i]` for all `1 <= i <= m`, where `&` represents the bitwise `AND` operator.

Return _the __minimum__ possible sum of the __values__ of the_ `m` _subarrays_ `nums` _is divided into_. _If it is not possible to divide_ `nums` _into_ `m` _subarrays satisfying these conditions, return_ `-1`.

__Example:__

Example 1:

```text
Input: nums = [1,4,3,3,2], andValues = [0,3,3,2]

Output: 12

Explanation:

The only possible way to divide nums is:

[1,4] as 1 & 4 == 0.
[3] as the bitwise AND of a single element subarray is that element itself.
[3] as the bitwise AND of a single element subarray is that element itself.
[2] as the bitwise AND of a single element subarray is that element itself.
```

The sum of the values for these subarrays is `4 + 3 + 3 + 2 = 12`.

Example 2:

```text
Input: nums = [2,3,5,7,7,7,5], andValues = [0,7,5]

Output: 17

Explanation:

There are three ways to divide nums:

[[2,3,5],[7,7,7],[5]] with the sum of the values 5 + 7 + 5 == 17.
[[2,3,5,7],[7,7],[5]] with the sum of the values 7 + 7 + 5 == 19.
[[2,3,5,7,7],[7],[5]] with the sum of the values 7 + 7 + 5 == 19.
```

The minimum possible sum of the values is `17`.

Example 3:

```text
Input: nums = [1,2,3,4], andValues = [2]

Output: -1

Explanation:
```

The bitwise `AND` of the entire array `nums` is `0`. As there is no possible way to divide `nums` into a single subarray to have the bitwise `AND` of elements `2`, return `-1`.

__Constraints:__

- `1 <= n == nums.length <= 10 ^ 4`
- `1 <= m == andValues.length <= min(n, 10)`
- `1 <= nums[i] < 10 ^ 5`
- `0 <= andValues[j] < 10 ^ 5`

__题目描述:__

给你两个数组 `nums` 和 `andValues`，长度分别为 `n` 和 `m`。

数组的 值 等于该数组的 最后一个 元素。

你需要将 `nums` 划分为 `m` 个 __不相交的连续__ 子数组，对于第 `i ^ th` 个子数组 `[li, ri]`，子数组元素的按位 `AND` 运算结果等于 `andValues[i]`，换句话说，对所有的 `1 <= i <= m`， `nums[li] & nums[li + 1] & ... & nums[ri] == andValues[i]` ，其中 `&` 表示按位 `AND` 运算符。

返回将 `nums` 划分为 `m` 个子数组所能得到的可能的 __最小__ 子数组 __值__ 之和。如果无法完成这样的划分，则返回 `-1` 。

__示例:__

示例 1：

```text
输入： nums = [1,4,3,3,2], andValues = [0,3,3,2]

输出： 12

解释：

唯一可能的划分方法为：

[1,4] 因为 1 & 4 == 0
[3] 因为单元素子数组的按位 AND 结果就是该元素本身
[3] 因为单元素子数组的按位 AND 结果就是该元素本身
[2] 因为单元素子数组的按位 AND 结果就是该元素本身
```

这些子数组的值之和为 `4 + 3 + 3 + 2 = 12`

示例 2：

```text
输入： nums = [2,3,5,7,7,7,5], andValues = [0,7,5]

输出： 17

解释：

划分 nums 的三种方式为:

[[2,3,5],[7,7,7],[5]] 其中子数组的值之和为 5 + 7 + 5 = 17
[[2,3,5,7],[7,7],[5]] 其中子数组的值之和为 7 + 7 + 5 = 19
[[2,3,5,7,7],[7],[5]] 其中子数组的值之和为 7 + 7 + 5 = 19
```

子数组值之和的最小可能值为 `17`

示例 3：

```text
输入： nums = [1,2,3,4], andValues = [2]

输出： -1

解释：
```

整个数组 `nums` 的按位 `AND` 结果为 `0`。由于无法将 `nums` 划分为单个子数组使得元素的按位 `AND` 结果为 `2`，因此返回 `-1`。

__提示：__

- `1 <= n == nums.length <= 10 ^ 4`
- `1 <= m == andValues.length <= min(n, 10)`
- `1 <= nums[i] < 10 ^ 5`
- `0 <= andValues[j] < 10 ^ 5`

__思路:__

```text
动态规划
设 dp[i][j][pre] 表示 nums[:i] 分成 j 个组其中前 j 个组都满足 andValues 的要求且当前组为 pre 的数目
设 m, n 分别为 andValues, nums 的大小
当 n - i < m - j 时不够分, 返回 inf 表示无法取到
当 j 为 m 时如果已经分完, 即 i 为 n, 返回 0, 否则返回 inf 表示无法取到
pre &= nums[i], 表示分给当前的组
然后分下一个组 dp[i + 1][j][pre]
如果 pre 和 andValues[j] 相等可以考虑分一个新组
初始化 pre = -1 求 dp[i + 1][j + 1][-1] + nums[i] 与当前的更小值
最后返回 dp[m][n][-1] 即可
如果不能组成合法的值返回 -1
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumValueSum(vector<int>& nums, vector<int>& andValues) 
    {
        int INF = 0x3f3f3f3f, n = nums.size(), m = andValues.size(), result = 0;
        unordered_map<long long, int> memo;
        auto dfs = [&](this auto&& dfs, int i, int j, int pre) -> int 
        {
            if (n - i < m - j) return INF;
            if (j == m) return i == n ? 0 : INF;
            long long mask = (long long) i << 36 | (long long) j << 32 | (pre &= nums[i]);
            if (memo.contains(mask)) return memo[mask];
            int cur = dfs(i + 1, j, pre); 
            if (pre == andValues[j]) cur = min(cur, dfs(i + 1, j + 1, -1) + nums[i]);
            return memo[mask] = cur; 
        };
        return ((result = dfs(0, 0, -1)) < INF) ? result : -1;
    }
};
```

__Java__:

```Java
class Solution {
    private Map<Long, Integer> memo = new HashMap<>();

    private int result;

    private int inf = 0x3f3f3f3f;

    public int minimumValueSum(int[] nums, int[] andValues) {
        return (result = dfs(0, 0, -1, nums, andValues)) < inf ? result : -1;
    }

    private int dfs(int i, int j, int pre, int[] nums, int[] andValues) {
        int n = nums.length, m = andValues.length;
        if (n - i < m - j) return inf;
        if (j == m) return i == n ? 0 : inf;
        long mask = (long) i << 36 | (long) j << 32 | (pre &= nums[i]);
        if (memo.containsKey(mask)) return memo.get(mask);
        int cur = dfs(i + 1, j, pre, nums, andValues);
        if (pre == andValues[j]) cur = Math.min(cur, dfs(i + 1, j + 1, -1, nums, andValues) + nums[i]);
        memo.put(mask, cur); 
        return cur;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumValueSum(self, nums: List[int], andValues: List[int]) -> int:
        m, n = len(andValues), len(nums)

        @lru_cache(None)
        def dfs(i: int, j: int, pre: int) -> int:
            if n - i < m - j:
                return inf
            if j == m:
                return 0 if i == n else inf
            result = dfs(i + 1, j, pre := pre & nums[i])
            if pre == andValues[j]:
                result = min(dfs(i + 1, j + 1, -1) + nums[i], result)
            return result
        return result if (result := dfs(0, 0, -1)) < inf else -1
```
