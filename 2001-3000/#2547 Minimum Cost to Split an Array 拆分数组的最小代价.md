# 2547 Minimum Cost to Split an Array 拆分数组的最小代价

__Description:__

You are given an integer array `nums` and an integer `k`.

Split the array into some number of non-empty subarrays. The cost of a split is the sum of the importance value of each subarray in the split.

Let `trimmed(subarray)` be the version of the subarray where all numbers which appear only once are removed.

- For example, `trimmed([3,1,2,4,3,4]) = [3,4,3,4].`

The __importance value__ of a subarray is `k + trimmed(subarray).length`.

- For example, if a subarray is `[1,2,3,3,3,4,4]`, then trimmed `([1,2,3,3,3,4,4]) = [3,3,3,4,4].`The importance value of this subarray will be `k + 5`.

Return _the minimum possible cost of a split of_ `nums`.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,2,1,2,1,3,3], k = 2
Output: 8
Explanation: We split nums to have two subarrays: [1,2], [1,2,1,3,3].
The importance value of [1,2] is 2 + (0) = 2.
The importance value of [1,2,1,3,3] is 2 + (2 + 2) = 6.
The cost of the split is 2 + 6 = 8. It can be shown that this is the minimum possible cost among all the possible splits.
```

Example 2:

```text
Input: nums = [1,2,1,2,1], k = 2
Output: 6
Explanation: We split nums to have two subarrays: [1,2], [1,2,1].
The importance value of [1,2] is 2 + (0) = 2.
The importance value of [1,2,1] is 2 + (2) = 4.
The cost of the split is 2 + 4 = 6. It can be shown that this is the minimum possible cost among all the possible splits.
```

Example 3:

```text
Input: nums = [1,2,1,2,1], k = 5
Output: 10
Explanation: We split nums to have one subarray: [1,2,1,2,1].
The importance value of [1,2,1,2,1] is 5 + (3 + 2) = 10.
The cost of the split is 10. It can be shown that this is the minimum possible cost among all the possible splits.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `0 <= nums[i] < nums.length`
- `1 <= k <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` 。

将数组拆分成一些非空子数组。拆分的 代价 是每个子数组中的 重要性 之和。

令 `trimmed(subarray)` 作为子数组的一个特征，其中所有仅出现一次的数字将会被移除。

- 例如， `trimmed([3,1,2,4,3,4]) = [3,4,3,4]` 。

子数组的 __重要性__ 定义为 `k + trimmed(subarray).length` 。

- 例如，如果一个子数组是 `[1,2,3,3,3,4,4]` ， `trimmed([1,2,3,3,3,4,4]) = [3,3,3,4,4]` 。这个子数组的重要性就是 `k + 5` 。

找出并返回拆分 `nums` 的所有可行方案中的最小代价。

子数组 是数组的一个连续 非空 元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,2,1,2,1,3,3], k = 2
输出：8
解释：将 nums 拆分成两个子数组：[1,2], [1,2,1,3,3]
[1,2] 的重要性是 2 + (0) = 2 。
[1,2,1,3,3] 的重要性是 2 + (2 + 2) = 6 。
拆分的代价是 2 + 6 = 8 ，可以证明这是所有可行的拆分方案中的最小代价。
```

示例 2：

```text
输入：nums = [1,2,1,2,1], k = 2
输出：6
解释：将 nums 拆分成两个子数组：[1,2], [1,2,1] 。
[1,2] 的重要性是 2 + (0) = 2 。
[1,2,1] 的重要性是 2 + (2) = 4 。
拆分的代价是 2 + 4 = 6 ，可以证明这是所有可行的拆分方案中的最小代价。
```

示例 3：

```text
输入：nums = [1,2,1,2,1], k = 5
输出：10
解释：将 nums 拆分成一个子数组：[1,2,1,2,1].
[1,2,1,2,1] 的重要性是 5 + (3 + 2) = 10 。
拆分的代价是 10 ，可以证明这是所有可行的拆分方案中的最小代价。
```

__提示：__

- `1 <= nums.length <= 1000`
- `0 <= nums[i] < nums.length`
- `1 <= k <= 10 ^ 9`

__思路:__

```text
1. 动态规划
设 dp[i + 1] 为前 i 个元素的最小代价
对于每个 i
倒序计算每个 j 为分割点, unique 为 [j, i] 中的唯一元素个数, visited 为 [j, i] 中每个元素的出现次数
如果 nums[j] 在 [j, i] 中首次出现, 即 visited[nums[j]] = 0, unique += 1
如果 nums[j] 在 [j, i] 中第二次出现, 即 visited[nums[j]] = 1, unique -= 1
如果 nums[j] 在 [j, i] 中第三次或更多次出现, 即 visited[nums[j]] >= 2, unique 不变
[j, i] 的重要性为 i - j + 1 - unique + k
dp[i + 1] = min(dp[j]) + i - j + 1 - unique + k
即 dp[i + 1] = i + 1 + k + min(dp[j]) - j - unique
初始值 dp[0] = 0
返回 dp[n]
注意到 dp[j] 每次都要减去 j
如果 dp2[i] = dp[i] - i
则 dp2[i + 1] = k + min(dp2[j]) - unique
dp2[n] = dp[n] - n
记 cur = min(cur, dp2[j] - unique)
则 dp2[i + 1] = k + cur
最后返回 dp2[n] + n
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
2. 线段树优化
记 last[nums[i]] 为 nums[i] 上一次出现的位置
记 last2[nums[i]] 为 nums[i] 上上一次出现的位置
则遍历到 i 时
将 [last[nums[i]] + 1, i] 区间的值加 1
将 [last2[nums[i]] + 1, last[nums[i]]] 区间的值减 1
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
const int MAX_N = 1000;
int mn[MAX_N * 4], todo[MAX_N * 4], last[MAX_N], last2[MAX_N];

class Solution 
{
    void do_(int o, int v) 
    {
        mn[o] += v;
        todo[o] += v;
    }

    void spread(int o) 
    {
        int v = todo[o];
        if (v) 
        {
            do_(o * 2, v);
            do_(o * 2 + 1, v);
            todo[o] = 0;
        }
    }

    void update(int o, int l, int r, int L, int R, int v) 
    {
        if (L <= l && r <= R) 
        {
            do_(o, v);
            return;
        }
        spread(o);
        int m = (l + r) / 2;
        if (m >= L) update(o * 2, l, m, L, R, v);
        if (m < R) update(o * 2 + 1, m + 1, r, L, R, v);
        mn[o] = min(mn[o * 2], mn[o * 2 + 1]);
    }

    int query(int o, int l, int r, int L, int R) 
    {
        if (L <= l && r <= R) return mn[o];
        spread(o);
        int m = (l + r) / 2;
        if (m >= R) return query(o * 2, l, m, L, R);
        if (m < L) return query(o * 2 + 1, m + 1, r, L, R);
        return min(query(o * 2, l, m, L, R), query(o * 2 + 1, m + 1, r, L, R));
    }

public:
    int minCost(vector<int> &nums, int k) 
    {
        int n = nums.size(), result = 0;
        memset(mn, 0, sizeof(int) * 4 * n);
        memset(todo, 0, sizeof(int) * 4 * n);
        memset(last, 0, sizeof(int) * n);
        memset(last2, 0, sizeof(int) * n);
        for (int i = 1; i <= n; i++) 
        {
            int x = nums[i - 1];
            update(1, 1, n, i, i, result);
            update(1, 1, n, last[x] + 1, i, -1);
            if (last[x]) update(1, 1, n, last2[x] + 1, last[x], 1);
            result = k + query(1, 1, n, 1, i);
            last2[x] = last[x];
            last[x] = i;
        }
        return result + n;
    }
};
```

__Java__:

```Java
class Solution {
    public int minCost(int[] nums, int k) {
        int n = nums.length, dp[] = new int[n + 1];
        for (int i = 0; i < n; i++) {
            int unique = 0, cur = Integer.MAX_VALUE, visited[] = new int[n];
            for (int j = i; j > -1; j--) {
                unique += visited[nums[j]] == 0 ? 1 : (visited[nums[j]] == 1 ? -1 : 0);
                ++visited[nums[j]];
                cur = Math.min(cur, dp[j] - unique);
            }
            dp[i + 1] = k + cur;
        }
        return dp[n] + n;
    }
}
```

__Python__:

```Python
class Solution:
    def minCost(self, nums: List[int], k: int) -> int:
        dp = [0] * ((n := len(nums)) + 1)
        for i in range(n):
            visited, unique, cur = [0] * n, 0, inf
            for j in range(i, -1, -1):
                unique += 1 if visited[nums[j]] == 0 else -1 if visited[nums[j]] == 1 else 0
                visited[nums[j]] += 1
                cur = min(cur, dp[j] - unique)
            dp[i + 1] = k + cur
        return dp[n] + n
```
