# 1000 Minimum Cost to Merge Stones 合并石头的最低成本

__Description__:
There are n piles of stones arranged in a row. The ith pile has stones[i] stones.

A move consists of merging exactly k consecutive piles into one pile, and the cost of this move is equal to the total number of stones in these k piles.

Return the minimum cost to merge all piles of stones into one pile. If it is impossible, return -1.

__Example:__

Example 1:

Input: stones = [3,2,4,1], k = 2
Output: 20
Explanation: We start with [3, 2, 4, 1].
We merge [3, 2] for a cost of 5, and we are left with [5, 4, 1].
We merge [4, 1] for a cost of 5, and we are left with [5, 5].
We merge [5, 5] for a cost of 10, and we are left with [10].
The total cost was 20, and this is the minimum possible.

Example 2:

Input: stones = [3,2,4,1], k = 3
Output: -1
Explanation: After any merge operation, there are 2 piles left, and we can't merge anymore.  So the task is impossible.

Example 3:

Input: stones = [3,5,1,2,6], k = 3
Output: 25
Explanation: We start with [3, 5, 1, 2, 6].
We merge [5, 1, 2] for a cost of 8, and we are left with [3, 8, 6].
We merge [3, 8, 6] for a cost of 17, and we are left with [17].
The total cost was 25, and this is the minimum possible.

__Constraints:__

n == stones.length
1 <= n <= 30
1 <= stones[i] <= 100
2 <= k <= 30

__题目描述__:
有 N 堆石头排成一排，第 i 堆中有 stones[i] 块石头。

每次移动（move）需要将连续的 K 堆石头合并为一堆，而这个移动的成本为这 K 堆石头的总数。

找出把所有石头合并成一堆的最低成本。如果不可能，返回 -1 。

__示例 :__

示例 1：

输入：stones = [3,2,4,1], K = 2
输出：20
解释：
从 [3, 2, 4, 1] 开始。
合并 [3, 2]，成本为 5，剩下 [5, 4, 1]。
合并 [4, 1]，成本为 5，剩下 [5, 5]。
合并 [5, 5]，成本为 10，剩下 [10]。
总成本 20，这是可能的最小值。

示例 2：

输入：stones = [3,2,4,1], K = 3
输出：-1
解释：任何合并操作后，都会剩下 2 堆，我们无法再进行合并。所以这项任务是不可能完成的。.

示例 3：

输入：stones = [3,5,1,2,6], K = 3
输出：25
解释：
从 [3, 5, 1, 2, 6] 开始。
合并 [5, 1, 2]，成本为 8，剩下 [3, 8, 6]。
合并 [3, 8, 6]，成本为 17，剩下 [17]。
总成本 25，这是可能的最小值。

__提示:__

1 <= stones.length <= 30
2 <= K <= 30
1 <= stones[i] <= 100

__思路__:

区间动态规划
题意是每次都必须拿出刚好 k 个元素合并, 所以先判断 (n - 1) % (k - 1) 是否为 0, 不能整除直接返回 -1
设 dp[i][j] 表示在 [i, j] 这个区间合并成 k 堆石头的最小成本
按照先枚举区间长度 len, 再枚举左端点 i, 最后枚举分界点 m
dp[i][j] = min(dp[i][j], dp[i][m] + dp[m + 1][j] + cost)
只有区间长度大小为 k 时才能合并, 即当 (len - 1) % (k - 1) == 0 时, dp[i][j] = dp[i][j] + pre[j + 1] - pre[i]
最后返回 dp[0][n - 1] 即可
初始化 dp[i][i] = stones[i], 因为当区间长度为 1 时, 最优解就是 stones[i] 本身
时间复杂度为 O(kn ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int mergeStones(vector<int>& stones, int k) 
    {
        int n = stones.size();
        if ((n - 1) % (k - 1)) return -1;
        vector<int> pre(n + 1);
        vector<vector<int>> dp(n, vector<int>(n));
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + stones[i];
        for (int l = 2; l <= n; l++)
        {
            for (int i = 0; i <= n - l; i++)
            {
                int j = i + l - 1;
                dp[i][j] = INT_MAX;
                for (int m = i; m < j; m += k - 1) dp[i][j] = min(dp[i][j], dp[i][m] + dp[m + 1][j]);
                if (!((l - 1) % (k - 1))) dp[i][j] += pre[j + 1] - pre[i];
            }
        }
        return dp.front().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int mergeStones(int[] stones, int k) {
        int n = stones.length, pre[] = new int[n + 1], dp[][] = new int[n][n];
        if ((n - 1) % (k - 1) != 0) return -1;
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + stones[i];
        for (int l = 2; l <= n; l++) {
            for (int i = 0; i <= n - l; i++) {
                int j = i + l - 1;
                dp[i][j] = Integer.MAX_VALUE;
                for (int m = i; m < j; m += k - 1) dp[i][j] = Math.min(dp[i][j], dp[i][m] + dp[m + 1][j]);
                if ((l - 1) % (k - 1) == 0) dp[i][j] += pre[j + 1] - pre[i];
            }
        }
        return dp[0][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def mergeStones(self, stones: List[int], k: int) -> int:
        n, prefix = len(stones), [0] + list(accumulate(stones))

        @lru_cache(None)
        def dp(i, j, m):
            return float('inf') if (j - i + 1 - m) % (k - 1) or (i == j and m != 1) else 0 if i == j and m == 1 else dp(i, j, k) + prefix[j + 1] - prefix[i] if m == 1 else min(dp(i, mid, 1) + dp(mid + 1, j, m - 1) for mid in range(i, j, k - 1))
        return result if (result := dp(0, n - 1, 1)) < float('inf') else -1
```
