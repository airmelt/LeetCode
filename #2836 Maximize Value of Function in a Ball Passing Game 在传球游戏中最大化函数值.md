# 2836 Maximize Value of Function in a Ball Passing Game 在传球游戏中最大化函数值

__Description:__

You are given an integer array `receiver` of length `n` and an integer `k`. `n` players are playing a ball-passing game.

You choose the starting player, `i`. The game proceeds as follows: player `i` passes the ball to player `receiver[i]`, who then passes it to `receiver[receiver[i]]`, and so on, for `k` passes in total. The game's score is the sum of the indices of the players who touched the ball, including repetitions, i.e. `i + receiver[i] + receiver[receiver[i]] + ... + receiver ^ (k)[i]`.

Return the maximum possible score.

Notes:

- `receiver` may contain duplicates.
- `receiver[i]` may be equal to `i`.

__Example:__

Example 1:

```text
Input: receiver = [2,0,1], k = 4

Output: 6

Explanation:
```

Starting with player `i = 2` the initial score is 2:

| Pass | Sender Index | Receiver Index | Score |
| ---- | ------------ | -------------- | ----- |
| 1 | 2 | 1 | 3 |
| 2 | 1 | 0 | 3 |
| 3 | 0 | 2 | 5 |
| 4 | 2 | 1 | 6 |

Example 2:

```text
Input: receiver = [1,1,1,2,3], k = 3

Output: 10

Explanation:
```

Starting with player `i = 4` the initial score is 4:

| Pass | Sender Index | Receiver Index | Score |
| ---- | ------------ | -------------- | ----- |
| 1 | 4 | 3 | 7 |
| 2 | 3 | 2 | 9 |
| 3 | 2 | 1 | 10 |

__Constraints:__

- `1 <= receiver.length == n <= 10 ^ 5`
- `0 <= receiver[i] <= n - 1`
- `1 <= k <= 10 ^ 10`

__题目描述:__

给你一个长度为 `n` 下标从 __0__ 开始的整数数组 `receiver` 和一个整数 `k` 。

总共有 `n` 名玩家，玩家 __编号__ 互不相同，且为 `[0, n - 1]` 中的整数。这些玩家玩一个传球游戏， `receiver[i]` 表示编号为 `i` 的玩家会传球给编号为 `receiver[i]` 的玩家。玩家可以传球给自己，也就是说 `receiver[i]` 可能等于 `i` 。

你需要从 `n` 名玩家中选择一名玩家作为游戏开始时唯一手中有球的玩家，球会被传 __恰好__ `k` 次。

如果选择编号为 `x` 的玩家作为开始玩家，定义函数 `f(x)` 表示从编号为 `x` 的玩家开始， `k` 次传球内所有接触过球玩家的编号之 __和__ ，如果有玩家多次触球，则 __累加多次__ 。换句话说， `f(x) = x + receiver[x] + receiver[receiver[x]] + ... + receiver ^ (k)[x]` 。

你的任务时选择开始玩家 `x` ，目的是 __最大化__ `f(x)` 。

请你返回函数的 最大值 。

__注意:__`receiver` 可能含有重复元素。

__示例:__

示例 1：

| 传递次数 | 传球者编号 | 接球者编号 | x + 所有接球者编号 |
| ------ | ------------ | -------------- | ----- |
| 1 | 2 | 1 | 3 |
| 2 | 1 | 0 | 3 |
| 3 | 0 | 2 | 5 |
| 4 | 2 | 1 | 6 |

```text
输入：receiver = [2,0,1], k = 4
输出：6
解释：上表展示了从编号为 x = 2 开始的游戏过程。
从表中可知，f(2) 等于 6 。
6 是能得到最大的函数值。
所以输出为 6 。
```

示例 2：

| 传递次数 | 传球者编号 | 接球者编号 | x + 所有接球者编号 |
| ------ | ------------ | -------------- | ----- |
| 1 | 4 | 3 | 7 |
| 2 | 3 | 2 | 9 |
| 3 | 2 | 1 | 10 |

```text
输入：receiver = [1,1,1,2,3], k = 3
输出：10
解释：上表展示了从编号为 x = 4 开始的游戏过程。
从表中可知，f(4) 等于 10 。
10 是能得到最大的函数值。
所以输出为 10 。
```

__提示：__

- `1 <= receiver.length == n <= 10 ^ 5`
- `0 <= receiver[i] <= n - 1`
- `1 <= k <= 10 ^ 10`

__思路:__

```text
倍增
记录下每个节点 x 到 2 ^ i 的节点以及得分
枚举节点 x 一直往上跳
时间复杂度为 O(NlogK), 空间复杂度为 O(NlogK)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long getMaxFunctionValue(vector<int>& receiver, long long k) 
    {
        int n = receiver.size(), m = 64 - __builtin_clzll(k);
        vector<vector<pair<int, long long>>> fa(m, vector<pair<int, long long>>(n));
        for (int i = 0; i < n; i++) fa.front()[i] = { receiver[i], receiver[i] };
        for (int i = 0; i < m - 1; i++) for (int x = 0; x < n; x++) fa[i + 1][x] = { fa[i][fa[i][x].first].first, fa[i][x].second + fa[i][fa[i][x].first].second }; 
        long long result = 0LL;
        for (int i = 0, x = i, ctz = 0; i < n; x = ++i) 
        {
            long long s = i;
            for (long long j = k; j; j &= j - 1LL) 
            {
                s += fa[ctz = __builtin_ctzll(j)][x].second;
                x = fa[ctz][x].first;
            }
            result = max(result, s);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long getMaxFunctionValue(List<Integer> receiver, long k) {
        int n = receiver.size(), m = 64 - Long.numberOfLeadingZeros(k), fa[][] = new int[m][n];
        long result = 0L, pre[][] = new long[m][n];
        for (int i = 0; i < n; i++) pre[0][i] = fa[0][i] = receiver.get(i);
        for (int i = 0; i < m - 1; i++) {
            for (int x = 0; x < n; x++) {
                fa[i + 1][x] = fa[i][fa[i][x]];
                pre[i + 1][x] = pre[i][x] + pre[i][fa[i][x]];
            }
        }
        for (int i = 0, x = i, ctz = 0; i < n; x = ++i) {
            long s = i;
            for (long j = k; j > 0L; j &= j - 1L) {
                s += pre[ctz = Long.numberOfTrailingZeros(j)][x];
                x = fa[ctz][x];
            }
            result = Math.max(result, s);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getMaxFunctionValue(self, receiver: List[int], k: int) -> int:
        n, m, result = len(receiver), k.bit_length() - 1, 0
        fa = [[(p, p)] + [None] * m for p in receiver]
        for i in range(m):
            for x in range(n):
                fa[x][i + 1] = (fa[fa[x][i][0]][i][0], fa[fa[x][i][0]][i][1] + fa[x][i][1])
        for i in range(n):
            x = s = i
            for j in range(m + 1):
                if (k >> j) & 1:
                    x, cur = fa[x][j]
                    s += cur
            result = max(result, s)
        return result
```
