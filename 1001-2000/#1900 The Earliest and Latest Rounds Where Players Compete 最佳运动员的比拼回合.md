# 1900 The Earliest and Latest Rounds Where Players Compete 最佳运动员的比拼回合

__Description:__

There is a tournament where `n` players are participating. The players are standing in a single row and are numbered from `1` to `n` based on their __initial__ standing position (player `1` is the first player in the row, player `2` is the second player in the row, etc.).

The tournament consists of multiple rounds (starting from round number `1`). In each round, the `i ^ th` player from the front of the row competes against the `i ^ th` player from the end of the row, and the winner advances to the next round. When the number of players is odd for the current round, the player in the middle automatically advances to the next round.

- For example, if the row consists of players `1, 2, 4, 6, 7`

  - Player `1` competes against player `7`.
  - Player `2` competes against player `6`.
  - Player `4` automatically advances to the next round.

After each round is over, the winners are lined back up in the row based on the original ordering assigned to them initially (ascending order).

The players numbered `firstPlayer` and `secondPlayer` are the best in the tournament. They can win against any other player before they compete against each other. If any two other players compete against each other, either of them might win, and thus you may __choose__ the outcome of this round.

Given the integers `n`, `firstPlayer`, and `secondPlayer`, return _an integer array containing two values, the __earliest__ possible round number and the __latest__ possible round number in which these two players will compete against each other, respectively_.

__Example:__

Example 1:

```text
Input: n = 11, firstPlayer = 2, secondPlayer = 4
Output: [3,4]
Explanation:
One possible scenario which leads to the earliest round number:
First round: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11
Second round: 2, 3, 4, 5, 6, 11
Third round: 2, 3, 4
One possible scenario which leads to the latest round number:
First round: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11
Second round: 1, 2, 3, 4, 5, 6
Third round: 1, 2, 4
Fourth round: 2, 4
```

Example 2:

```text
Input: n = 5, firstPlayer = 1, secondPlayer = 5
Output: [1,1]
Explanation: The players numbered 1 and 5 compete in the first round.
There is no way to make them compete in any other round.
```

__Constraints:__

- `2 <= n <= 28`
- `1 <= firstPlayer < secondPlayer <= n`

__题目描述:__

`n` 名运动员参与一场锦标赛，所有运动员站成一排，并根据 __最开始的__ 站位从 `1` 到 `n` 编号（运动员 `1` 是这一排中的第一个运动员，运动员 `2` 是第二个运动员，依此类推）。

锦标赛由多个回合组成（从回合 `1` 开始）。每一回合中，这一排从前往后数的第 `i` 名运动员需要与从后往前数的第 `i` 名运动员比拼，获胜者将会进入下一回合。如果当前回合中运动员数目为奇数，那么中间那位运动员将轮空晋级下一回合。

- 例如，当前回合中，运动员 `1, 2, 4, 6, 7` 站成一排

  - 运动员 `1` 需要和运动员 `7` 比拼
  - 运动员 `2` 需要和运动员 `6` 比拼
  - 运动员 `4` 轮空晋级下一回合

每回合结束后，获胜者将会基于最开始分配给他们的原始顺序（升序）重新排成一排。

编号为 `firstPlayer` 和 `secondPlayer` 的运动员是本场锦标赛中的最佳运动员。在他们开始比拼之前，完全可以战胜任何其他运动员。而任意两个其他运动员进行比拼时，其中任意一个都有获胜的可能，因此你可以 __裁定__ 谁是这一回合的获胜者。

给你三个整数 `n`、 `firstPlayer` 和 `secondPlayer` 。返回一个由两个值组成的整数数组，分别表示两位最佳运动员在本场锦标赛中比拼的 __最早__ 回合数和 __最晚__ 回合数。

__示例:__

示例 1：

```text
输入：n = 11, firstPlayer = 2, secondPlayer = 4
输出：[3,4]
解释：
一种能够产生最早回合数的情景是：
回合 1：1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11
回合 2：2, 3, 4, 5, 6, 11
回合 3：2, 3, 4
一种能够产生最晚回合数的情景是：
回合 1：1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11
回合 2：1, 2, 3, 4, 5, 6
回合 3：1, 2, 4
回合 4：2, 4
```

示例 2：

```text
输入：n = 5, firstPlayer = 1, secondPlayer = 5
输出：[1,1]
解释：两名最佳运动员 1 和 5 将会在回合 1 进行比拼。
不存在使他们在其他回合进行比拼的可能。
```

__提示：__

- `2 <= n <= 28`
- `1 <= firstPlayer < secondPlayer <= n`

__思路:__

```text
1. 状态压缩 ➕ 对称
用一个整形来表示当前留下来的所有运动员
1 表示当前运动员留下来，0 表示当前运动员被淘汰
计算 1 出现的次数 n, 即为当前轮次运动员的数量
如果 n < 2, 或者运动员中不包含 firstPlayer 或 secondPlayer, 则返回
记录时只需要记录前一半的运动员, 后一半的运动员可以通过对称性得到
如果 firstPlayer 和 secondPlayer 相遇, 则更新最早回合数和最晚回合数
当运动员数量为奇数时, 中间的运动员自动晋级下一轮, 直接把中间的运动员加到状态中即可
时间复杂度为 O(2 ^ N), 空间复杂度为 O(N)
2. 动态规划 ➕ 对称
设 F[n][i][j] 表示当前轮次为 n, firstPlayer 位于 i, secondPlayer 位于 j 的最早回合数
设 G[n][i][j] 表示当前轮次为 n, firstPlayer 位于 i, secondPlayer 位于 j 的最晚回合数
交换两个运动员的顺序, 结果不会改变, 即 F[n][i][j] = F[n][j][i]
翻转所有运动员的顺序, 结果不会改变, 即 F[n][i][j] = F[n][n + 1 - i][n + 1 - j]
所以我们只需要检查 i < mid 的情况即可
转移方程 F[n][i][j] = min(F[n][i][j], F[(n + 1) >> 1][i + 1][i + j + mid + 2] + 1)
G 的转移方程把 min 改为 max 即可
时间复杂度为 O(N ^ 4logN), 空间复杂度为 O(N ^ 3)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> earliestAndLatest(int n, int firstPlayer, int secondPlayer) 
    {
        helper(n, firstPlayer, secondPlayer, 0);
        return {first, second};
    }
private:
    int first = INT_MAX, second = INT_MIN;

    void helper(int n, int firstPlayer, int secondPlayer, int depth) {
        int d = depth + 1, mid = (n >> 1) + 1;
        if (firstPlayer + secondPlayer == n + 1) 
        {
            first = min(first, depth + 1);
            second = max(second, depth + 1);
            return;
        }
        if (firstPlayer + secondPlayer > n) return helper(n, n + 1 - secondPlayer, n + 1 - firstPlayer, depth);
        if (secondPlayer >= mid) for (int i = 0; i < firstPlayer; i++) for (int j = 0; j <= n - firstPlayer - secondPlayer; j++) helper((n + 1) >> 1, i + 1, i + 1 + j + secondPlayer - mid + 1, d);
        else for (int i = 0; i < firstPlayer; i++) for (int j = 0; j <= secondPlayer - firstPlayer - 1; j++) helper((n + 1) >> 1, i + 1, i + 1 + j + 1, d);
    }
};
```

__Java__:

```Java
class Solution {
    private int a = 0, b = 0, first = Integer.MAX_VALUE, second = Integer.MIN_VALUE;

    public int[] earliestAndLatest(int n, int firstPlayer, int secondPlayer) {
        int state = (1 << n) - 1;
        a = firstPlayer - 1;
        b = secondPlayer - 1;
        dfs(state, 1);
        return new int[]{first, second};
    }

    private void dfs(int state, int rounds) {
        int n = helper(state), t = n >> 1, cur = 0, q[] = new int[29];
        if (n < 2 || ((state >> a) & 1) == 0 || ((state >> b) & 1) == 0) return;
        for (int i = 0; i < 29; i++) if (((state >> i) & 1) != 0) q[cur++] = i;
        for (int mask = 0; mask < (1 << t); mask++) {
            int nextState = 0;
            for (int i = 0; i < t; i++) {
                if (((mask >> i) & 1) != 0) nextState |= (1 << q[i]);
                else nextState |= (1 << q[n - i - 1]);
                if (q[i] == a && q[n - i - 1] == b || q[i] == b && q[n - i - 1] == a) {
                    first = Math.min(first, rounds);
                    second = Math.max(second, rounds);
                    return;
                }
            }
            if ((n & 1) != 0) nextState |= 1 << q[t];
            dfs(nextState, rounds + 1);
        }
    }

    private int helper(int n) {
        int result = 0;
        while (n > 0) {
            ++result;
            n &= (n - 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def earliestAndLatest(self, n: int, firstPlayer: int, secondPlayer: int) -> List[int]:
        @lru_cache(None)
        def dfs(cur: List[int]) -> List[int]:
            n, result = len(cur), [inf, -inf]
            group = [(cur[i], cur[n - i - 1]) for i in range((n + 1) >> 1)]
            if (firstPlayer, secondPlayer) in group:
                return [1, 1]
            for pair in product(*group):
                if firstPlayer in pair and secondPlayer in pair:
                    result = [min(result[0], 1 + (r := dfs(tuple(sorted(pair))))[0]), max(result[1], 1 + r[1])]
            return result
        return dfs(tuple(range(1, n + 1)))
```
