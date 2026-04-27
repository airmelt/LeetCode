# 1547 Minimum Cost to Cut a Stick 切棍子的最小成本

__Description:__

Given a wooden stick of length `n` units. The stick is labelled from `0` to `n`. For example, a stick of length __6__ is labelled as follows:

![1547-1](https://assets.leetcode.com/uploads/2020/07/21/statement.jpg)

Given an integer array `cuts` where `cuts[i]` denotes a position you should perform a cut at.

You should perform the cuts in order, you can change the order of the cuts as you wish.

The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. When you cut a stick, it will be split into two smaller sticks (i.e. the sum of their lengths is the length of the stick before the cut). Please refer to the first example for a better explanation.

Return the minimum total cost of the cuts.

__Example:__

Example 1:

![1547-2](https://assets.leetcode.com/uploads/2020/07/23/e1.jpg)

```text
Input: n = 7, cuts = [1,3,4,5]
Output: 16
Explanation: Using cuts order = [1, 3, 4, 5] as in the input leads to the following scenario:

The first cut is done to a rod of length 7 so the cost is 7. The second cut is done to a rod of length 6 (i.e. the second part of the first cut), the third is done to a rod of length 4 and the last cut is to a rod of length 3. The total cost is 7 + 6 + 4 + 3 = 20.
Rearranging the cuts to be [3, 5, 1, 4] for example will lead to a scenario with total cost = 16 (as shown in the example photo 7 + 4 + 3 + 2 = 16).
```

![1547-3](https://assets.leetcode.com/uploads/2020/07/21/e11.jpg)

Example 2:

```text
Input: n = 9, cuts = [5,6,1,4,2]
Output: 22
Explanation: If you try the given cuts ordering the cost will be 25.
There are much ordering with total cost <= 25, for example, the order [4, 6, 5, 2, 1] has total cost = 22 which is the minimum possible.
```

__Constraints:__

- `2 <= n <= 10 ^ 6`
- `1 <= cuts.length <= min(n - 1, 100)`
- `1 <= cuts[i] <= n - 1`
- All the integers in `cuts` array are __distinct__.

__题目描述:__

有一根长度为 `n` 个单位的木棍，棍上从 `0` 到 `n` 标记了若干位置。例如，长度为 __6__ 的棍子可以标记如下：

![1547-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/09/statement.jpg)

给你一个整数数组 `cuts` ，其中 `cuts[i]` 表示你需要将棍子切开的位置。

你可以按顺序完成切割，也可以根据需要更改切割的顺序。

__示例:__

每次切割的成本都是当前要切割的棍子的长度，切棍子的总成本是历次切割成本的总和。对棍子进行切割将会把一根木棍分成两根较小的木棍（这两根木棍的长度和就是切割前木棍的长度）。请参阅第一个示例以获得更直观的解释。

```text
返回切棍子的 最小总成本 。
```

示例 1：

![1547-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/09/e1.jpg)

输入：n = 7, cuts = [1,3,4,5]
输出：16
解释：按 [1, 3, 4, 5] 的顺序切割的情况如下所示：

第一次切割长度为 7 的棍子，成本为 7 。第二次切割长度为 6 的棍子（即第一次切割得到的第二根棍子），第三次切割为长度 4 的棍子，最后切割长度为 3 的棍子。总成本为 7 + 6 + 4 + 3 = 20 。
而将切割顺序重新排列为 [3, 5, 1, 4] 后，总成本 = 16（如示例图中 7 + 4 + 3 + 2 = 16）。

![1547-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/09/e11.jpg)

示例 2：

```text
输入：n = 9, cuts = [5,6,1,4,2]
输出：22
解释：如果按给定的顺序切割，则总成本为 25 。总成本 <= 25 的切割顺序很多，例如，[4, 6, 5, 2, 1] 的总成本 = 22，是所有可能方案中成本最小的。
```

__提示：__

- `2 <= n <= 10 ^ 6`
- `1 <= cuts.length <= min(n - 1, 100)`
- `1 <= cuts[i] <= n - 1`
- `cuts` 数组中的所有整数都 __互不相同__

__思路:__

```text
动态规划
设 dp[i][j] 表示切割区间为 [i...j] 的最小成本
最后返回 dp[0][m] 即可
dp[i][j] = min(dp[i][k - 1] + dp[k + 1][j] + cuts[j + 1] - cuts[i - 1]), 其中 i <= k <= j
为了方便计算, 在 cuts 数组前加入 0, 后加入 n 作为哨兵
时间复杂度为 O(M ^ 3), 空间复杂度为 O(M ^ 2), M 为 cuts 数组的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minCost(int n, vector<int>& cuts) 
    {
        int m = cuts.size(), dp[102][102];
        memset(dp, 0, sizeof dp);
        sort(cuts.begin(), cuts.end());
        cuts.insert(cuts.begin(), 0);
        cuts.emplace_back(n);
        for (int i = 1; i < m + 2; i++) 
        {
            for (int j = 0; j < m + 2 - i; j++) 
            {
                int p = j + i, cur = INT_MAX;
                for (int k = j + 1; k < p; k++) cur = min(dp[j][k] + dp[k][p] + cuts[p] - cuts[j], cur);
                if (cur < INT_MAX) dp[j][p] = cur;
            }
        }
        return dp[0][m + 1];
    }
};
```

__Java__:

```Java
class Solution {
    public int minCost(int n, int[] cuts) {
        int m = cuts.length, dp[][] = new int[102][102], cut[] = new int[m + 2];
        Arrays.sort(cuts);
        for (int i = 1; i <= m; i++) cut[i] = cuts[i - 1];
        cut[m + 1] = n;
        for (int i = 1; i < m + 2; i++) {
            for (int j = 0; j < m + 2 - i; j++) {
                int p = j + i, cur = Integer.MAX_VALUE;
                for (int k = j + 1; k < p; k++) cur = Math.min(dp[j][k] + dp[k][p] + cut[p] - cut[j], cur);
                if (cur < Integer.MAX_VALUE) dp[j][p] = cur;
            }
        }
        return dp[0][m + 1];
    }
}
```

__Python__:

```Python
class Solution:
    def minCost(self, n: int, cuts: List[int]) -> int:
        cuts.sort()
        dp, cuts, n = [[0] * 102 for _ in range(102)], [0] + cuts + [n], len(cuts)
        for i in range(1, n + 2):
            for j in range(n + 2 - i):
                p, cur = j + i, float('inf')
                for k in range(j + 1, p):
                    cur = min(dp[j][k] + dp[k][p] + cuts[p] - cuts[j], cur)
                if cur < float('inf'):
                    dp[j][p] = cur
        return dp[0][n + 1]
```
