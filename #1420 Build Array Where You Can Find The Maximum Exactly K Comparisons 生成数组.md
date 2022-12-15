# 1420 Build Array Where You Can Find The Maximum Exactly K Comparisons 生成数组

__Description:__

You are given three integers `n`, `m` and `k`. Consider the following algorithm to find the maximum element of an array of positive integers:

![1420-1](https://assets.leetcode.com/uploads/2020/04/02/e.png)

You should build the array arr which has the following properties:

- `arr` has exactly `n` integers.
- `1 <= arr[i] <= m` where `(0 <= i < n)`.
- After applying the mentioned algorithm to `arr`, the value `search_cost` is equal to `k`.

Return _the number of ways_ to build the array `arr` under the mentioned conditions. As the answer may grow large, the answer __must be__ computed modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: n = 2, m = 3, k = 1
Output: 6
Explanation: The possible arrays are [1, 1], [2, 1], [2, 2], [3, 1], [3, 2] [3, 3]
```

Example 2:

```text
Input: n = 5, m = 2, k = 3
Output: 0
Explanation: There are no possible arrays that satisify the mentioned conditions.
```

Example 3:

```text
Input: n = 9, m = 1, k = 1
Output: 1
Explanation: The only possible array is [1, 1, 1, 1, 1, 1, 1, 1, 1]
```

__Constraints:__

- `1 <= n <= 50`
- `1 <= m <= 100`
- `0 <= k <= n`

__题目描述:__

给你三个整数 `n`、`m` 和 `k` 。下图描述的算法用于找出正整数数组中最大的元素。

![1420-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/04/19/e.png)

请你生成一个具有下述属性的数组 `arr` ：

- `arr` 中有 `n` 个整数。
- `1 <= arr[i] <= m` 其中 `(0 <= i < n)` 。
- 将上面提到的算法应用于 `arr` ，`search_cost` 的值等于 `k` 。

返回上述条件下生成数组 `arr` 的 __方法数__ ，由于答案可能会很大，所以 __必须__ 对 `10 ^ 9 + 7` 取余。

__示例:__

示例 1：

```text
输入：n = 2, m = 3, k = 1
输出：6
解释：可能的数组分别为 [1, 1], [2, 1], [2, 2], [3, 1], [3, 2] [3, 3]
```

示例 2：

```text
输入：n = 5, m = 2, k = 3
输出：0
解释：没有数组可以满足上述条件
```

示例 3：

```text
输入：n = 9, m = 1, k = 1
输出：1
解释：可能的数组只有 [1, 1, 1, 1, 1, 1, 1, 1, 1]
```

示例 4：

```text
输入：n = 50, m = 100, k = 25
输出：34549172
解释：不要忘了对 1000000007 取余
```

示例 5：

```text
输入：n = 37, m = 17, k = 7
输出：418930126
```

__提示：__

- `1 <= n <= 50`
- `1 <= m <= 100`
- `0 <= k <= n`

__思路:__

```text
动态规划 ➕ 前缀和
从题目的数据量推断尝试使用动态规划
设 dp[p][j][i] 表示长度为 i 的数组最大值为 j, cost 为 p
最后需要返回 sum(dp[k][j][n]), 其中 1 <= j <= m
1. dp[p][j][i] = dp[p][j][i - 1] * j, 如果第 i 个数没有改变数组的最大值, 那么第 i 个数的取值范围为 [1, j]
2. dp[p][j][i] = sum(dp[p - 1][q][i - 1]), 如果第 i 个数改变了数组的最大值, 需要消耗一点 cost, q 的取值范围为 [1, j - 1], 严格小于 j
所以转移方程为 dp[p][j][i] = dp[p][j][i - 1] * j + sum(dp[p - 1][q][i - 1])
初始化 dp[1][j][1] = 1, 因为每次最大值从 0 初始化, 到达第一个数字时必消耗一点 cost
从上述分析首先可以看到第 i 种情况只取决于第 i - 1 种情况, 可以用滚动数组压缩空间复杂度
另外在第二种情况中出现了求和函数, 那么可以考虑使用前缀和优化避免重复计算
简化后的转移方程为 cur[p][j] = pre[p][j] * j + pre_sum
其中 pre_sum = sum(pre[j - 1][x]), 即前缀和
这里还可以使用剪枝加快运算速度, cost 取 min(k, i) 才有意义
时间复杂度为 O(NMK), 空间复杂度为 O(MK)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numOfArrays(int n, int m, int k) 
    {
        vector<vector<long>> cur(k + 1, vector<long>(m + 1)), pre(k + 1, vector<long>(m + 1)), zero(k + 1, vector<long>(m + 1));
        long mod = 1e9 + 7, pre_sum = 0;
        for (int i = 1; i <= m; i++) pre[1][i] = 1;
        for (int i = 2; i <= n; i++)
        {
            for (int p = 1; p <= min(k, i); p++)
            {
                pre_sum = 0;
                for (int j = 1; j <= m; j++)
                {
                    cur[p][j] = (pre[p][j] * j + pre_sum) % mod;
                    pre_sum += pre[p - 1][j];
                }
            }
            pre = cur;
            cur = zero;
        }
        return accumulate(pre.back().begin(), pre.back().end(), 0L) % mod;
    }
};
```

__Java__:

```Java
class Solution 
{
public:
    int numOfArrays(int n, int m, int k) 
    {
        vector<vector<long>> cur(k + 1, vector<long>(m + 1)), pre(k + 1, vector<long>(m + 1)), zero(k + 1, vector<long>(m + 1));
        long mod = 1e9 + 7, pre_sum = 0;
        for (int i = 1; i <= m; i++) pre[1][i] = 1;
        for (int i = 2; i <= n; i++)
        {
            for (int j = 1; j <= min(k, i); j++)
            {
                pre_sum = 0;
                for (int x = 1; x <= m; x++)
                {
                    cur[j][x] = (pre[j][x] * x + pre_sum) % mod;
                    pre_sum += pre[j - 1][x];
                }
            }
            pre = cur;
            cur = zero;
        }
        return accumulate(pre.back().begin(), pre.back().end(), 0L) % mod;
    }
};
```

__Python__:

```Python
class Solution 
{
public:
    int numOfArrays(int n, int m, int k) 
    {
        vector<vector<long>> cur(k + 1, vector<long>(m + 1)), pre(k + 1, vector<long>(m + 1)), zero(k + 1, vector<long>(m + 1));
        long mod = 1e9 + 7, pre_sum = 0;
        for (int i = 1; i <= m; i++) pre[1][i] = 1;
        for (int i = 2; i <= n; i++)
        {
            for (int j = 1; j <= min(k, i); j++)
            {
                pre_sum = 0;
                for (int x = 1; x <= m; x++)
                {
                    cur[j][x] = (pre[j][x] * x + pre_sum) % mod;
                    pre_sum += pre[j - 1][x];
                }
            }
            pre = cur;
            cur = zero;
        }
        return accumulate(pre.back().begin(), pre.back().end(), 0L) % mod;
    }
};
```
