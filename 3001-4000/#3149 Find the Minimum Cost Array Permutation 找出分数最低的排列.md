# 3149 Find the Minimum Cost Array Permutation 找出分数最低的排列

__Description:__

You are given an array `nums` which is a __permutation__ of `[0, 1, 2, ..., n - 1]`. The __score__ of any permutation of `[0, 1, 2, ..., n - 1]` named perm is defined as:

`score(perm) = |perm[0] - nums[perm[1]]| + |perm[1] - nums[perm[2]]| + ... + |perm[n - 1] - nums[perm[0]]|`

Return the permutation `perm` which has the __minimum__ possible score. If _multiple_ permutations exist with this score, return the one that is __lexicographically smallest__ among them.

__Example:__

Example 1:

![3149-1](https://assets.leetcode.com/uploads/2024/04/04/example0gif.gif)

```text
Input: nums = [1,0,2]

Output: [0,1,2]

Explanation:

The lexicographically smallest permutation with minimum cost is [0,1,2]. The cost of this permutation is |0 - 0| + |1 - 2| + |2 - 1| = 2.
```

Example 2:

![3149-2](https://assets.leetcode.com/uploads/2024/04/04/example1gif.gif)

```text
Input: nums = [0,2,1]

Output: [0,2,1]

Explanation:

The lexicographically smallest permutation with minimum cost is [0,2,1]. The cost of this permutation is |0 - 1| + |2 - 2| + |1 - 0| = 2.
```

__Constraints:__

- `2 <= n == nums.length <= 14`
- `nums` is a permutation of `[0, 1, 2, ..., n - 1]`.

__题目描述:__

给你一个数组 `nums` ，它是 `[0, 1, 2, ..., n - 1]` 的一个 __排列__ 。对于任意一个 `[0, 1, 2, ..., n - 1]` 的排列 `perm` ，其 __分数__ 定义为：

`score(perm) = |perm[0] - nums[perm[1]]| + |perm[1] - nums[perm[2]]| + ... + |perm[n - 1] - nums[perm[0]]|`

返回具有 __最低__ 分数的排列 `perm` 。如果存在多个满足题意且分数相等的排列，则返回其中 __字典序最小__ 的一个。

__示例:__

示例 1：

![3149-3](https://assets.leetcode.com/uploads/2024/04/04/example0gif.gif)

```text
输入：nums = [1,0,2]

输出：[0,1,2]

解释：

字典序最小且分数最低的排列是 [0,1,2]。这个排列的分数是 |0 - 0| + |1 - 2| + |2 - 1| = 2 。
```

示例 2：

![3149-4](https://assets.leetcode.com/uploads/2024/04/04/example1gif.gif)

```text
输入：nums = [0,2,1]

输出：[0,2,1]

解释：

字典序最小且分数最低的排列是 [0,2,1]。这个排列的分数是 |0 - 1| + |2 - 2| + |1 - 0| = 2 。
```

__提示：__

- `2 <= n == nums.length <= 14`
- `nums` 是 `[0, 1, 2, ..., n - 1]` 的一个排列。

__思路:__

```text
状态压缩 ➕ 动态规划
注意到答案都是 0 开头
猜想答案必须以 0 为开头
尝试循环左移答案, 会发现分数不变
设 dp[s][j] 表示当前选择的集合为 s, 最后一次选择的数为 j 的最小分数
令 n 为 nums 的长度, total = 1 << n
最后会选择所有的数, 并且选择任意一个数 j 的分数为 abs(j - nums[0])
所以可以初始化 dp[-1][j] = abs(j - nums[0])
dp[s][j] = min(dp[s | (1 << k)][k] + abs(j - nums[k])), 其中 k 为 (0, n) 中的整数, 并且满足 k 不在 s 中, 即 s >> k & 1 为 0
每次选择 k 时如果能更新 dp[s][j], 那么也记录 path[s][j]
最后从 0 出发, 遍历 path[0][0] 直到找到所有的数字
时间复杂度为 O(N ^ 2 * 2 ^ N), 空间复杂度为 O(N * 2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findPermutation(vector<int>& nums) 
    {
        int n = nums.size(), total = 1 << n, inf = 0x3f3f3f3f, a = 0, b = 0;
        vector<vector<int>> dp(total, vector<int>(n, inf)), path(total, vector<int>(n, -1));
        for (int i = 0; i < n; i++) dp.back()[i] = abs(i - nums.front());
        for (int s = total - 3; s > 0; s -= 2)
        {
            for (int j = 0; j < n; j++)
            {
                if (s >> j & 1)
                {
                    for (int k = 1, v = 0; k < n; k++)
                    {
                        if (!(s >> k & 1) and (v = dp[s | (1 << k)][k] + abs(j - nums[k])) < dp[s][j])
                        {
                            dp[s][j] = v;
                            path[s][j] = k;
                        }
                    }
                }
            }
        }
        vector<int> result;
        while (b > -1)
        {
            result.emplace_back(b);
            a |= 1 << b;
            b = path[a][b];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findPermutation(int[] nums) {
        int n = nums.length, total = 1 << n, inf = 0x3f3f3f3f, dp[][] = new int[total][n], path[][] = new int[total][n], result[] = new int[n], a = 0, b = 0, c = 0;
        result[0] = 0;
        for (var row : dp) Arrays.fill(row, inf);
        Arrays.fill(path[total - 1], -1);
        for (int i = 0; i < n; i++) dp[total - 1][i] = Math.abs(i - nums[0]);
        for (int s = total - 3; s > 0; s -= 2) {
            for (int j = 0; j < n; j++) {
                if ((s >> j & 1) != 0) {
                    for (int k = 1, v = 0; k < n; k++) {
                        if ((s >> k & 1) == 0 && (v = dp[s | (1 << k)][k] + Math.abs(j - nums[k])) < dp[s][j]) {
                            dp[s][j] = v;
                            path[s][j] = k;
                        }
                    }
                }
            }
        }
        while (b > -1) {
            result[c++] = b;
            a |= 1 << b;
            b = path[a][b];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findPermutation(self, nums: List[int]) -> List[int]:
        n = len(nums)
        dp, path, result = [[inf] * n for _ in range(1 << n)], [[-1] * n for _ in range(1 << n)], []
        for i in range(n):
            dp[-1][i] = abs(i - nums[0])
        for s in range((1 << n) - 3, 0, -2):
            for j in range(n):
                if s >> j & 1:
                    for k in range(1, n):
                        if not s >> k & 1 and (v := dp[s | (1 << k)][k] + abs(j - nums[k])) < dp[s][j]:
                            dp[s][j], path[s][j] = v, k
        s = j = 0
        while j > -1:
            result.append(j)
            s |= 1 << j
            j = path[s][j]
        return result
```
