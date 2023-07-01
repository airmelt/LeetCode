# 1690 Stone Game VII 石子游戏VII

__Description:__

Alice and Bob take turns playing a game, with Alice starting first.

There are `n` stones arranged in a row. On each player's turn, they can __remove__ either the leftmost stone or the rightmost stone from the row and receive points equal to the __sum__ of the remaining stones' values in the row. The winner is the one with the higher score when there are no stones left to remove.

Bob found that he will always lose this game (poor Bob, he always loses), so he decided to minimize the score's difference. Alice's goal is to maximize the difference in the score.

Given an array of integers `stones` where `stones[i]` represents the value of the `i ^ th` stone __from the left__, return _the __difference__ in Alice and Bob's score if they both play __optimally__._

__Example:__

Example 1:

```text
Input: stones = [5,3,1,4,2]
Output: 6
Explanation: 
- Alice removes 2 and gets 5 + 3 + 1 + 4 = 13 points. Alice = 13, Bob = 0, stones = [5,3,1,4].
- Bob removes 5 and gets 3 + 1 + 4 = 8 points. Alice = 13, Bob = 8, stones = [3,1,4].
- Alice removes 3 and gets 1 + 4 = 5 points. Alice = 18, Bob = 8, stones = [1,4].
- Bob removes 1 and gets 4 points. Alice = 18, Bob = 12, stones = [4].
- Alice removes 4 and gets 0 points. Alice = 18, Bob = 12, stones = [].
The score difference is 18 - 12 = 6.
```

Example 2:

```text
Input: stones = [7,90,5,1,100,10,10,2]
Output: 122
```

__Constraints:__

- `n == stones.length`
- `2 <= n <= 1000`
- `1 <= stones[i] <= 1000`

__题目描述:__

石子游戏中，爱丽丝和鲍勃轮流进行自己的回合，爱丽丝先开始 。

有 `n` 块石子排成一排。每个玩家的回合中，可以从行中 __移除__ 最左边的石头或最右边的石头，并获得与该行中剩余石头值之 __和__ 相等的得分。当没有石头可移除时，得分较高者获胜。

鲍勃发现他总是输掉游戏（可怜的鲍勃，他总是输），所以他决定尽力 减小得分的差值 。爱丽丝的目标是最大限度地 扩大得分的差值 。

给你一个整数数组 `stones` ，其中 `stones[i]` 表示 __从左边开始__ 的第 `i` 个石头的值，如果爱丽丝和鲍勃都 __发挥出最佳水平__ ，请返回他们 __得分的差值__ 。

__示例:__

示例 1：

```text
输入：stones = [5,3,1,4,2]
输出：6
解释：
- 爱丽丝移除 2 ，得分 5 + 3 + 1 + 4 = 13 。游戏情况：爱丽丝 = 13 ，鲍勃 = 0 ，石子 = [5,3,1,4] 。
- 鲍勃移除 5 ，得分 3 + 1 + 4 = 8 。游戏情况：爱丽丝 = 13 ，鲍勃 = 8 ，石子 = [3,1,4] 。
- 爱丽丝移除 3 ，得分 1 + 4 = 5 。游戏情况：爱丽丝 = 18 ，鲍勃 = 8 ，石子 = [1,4] 。
- 鲍勃移除 1 ，得分 4 。游戏情况：爱丽丝 = 18 ，鲍勃 = 12 ，石子 = [4] 。
- 爱丽丝移除 4 ，得分 0 。游戏情况：爱丽丝 = 18 ，鲍勃 = 12 ，石子 = [] 。
得分的差值 18 - 12 = 6 。
```

示例 2：

```text
输入：stones = [7,90,5,1,100,10,10,2]
输出：122
```

__提示：__

- `n == stones.length`
- `2 <= n <= 1000`
- `1 <= stones[i] <= 1000`

__思路:__

```text
动态规划 ➕ 前缀和
设 dp[i][j] 表示在区间 [i, j] 的最大得分差, 即先手的总分和后手的总分之差
最后返回 dp[0][n - 1] 即可
对于区间 [i, j]
如果移除左边的元素, 即 sum(stones[i + 1:j]) - dp[i + 1][j]
如果移除右边的元素, 即 sum(stones[i:j - 1]) - dp[i][j - 1]
所以 dp[i][j] = max(sum(stones[i + 1:j]) - dp[i + 1][j], sum(stones[i:j - 1]) - dp[i][j - 1])
定义前缀和 pre[i] 表示 stones[:i] 的和
则 sum(stones[i + 1:j]) = pre[j + 1] - pre[i + 1], sum(stones[i:j - 1]) = pre[j] - pre[i]
所以 dp[i][j] = max(pre[j + 1] - pre[i + 1] - dp[i + 1][j], pre[j] - pre[i] - dp[i][j - 1])
边界条件为 dp[i][i] = 0, 边界长度为 0 时得分为 0
注意到 dp[i][j] 只与 dp[i + 1][j] 和 dp[i][j - 1] 有关, 所以可以使用滚动数组优化空间复杂度
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int stoneGameVII(vector<int>& stones) 
    {
        int n = stones.size();
        vector<int> dp(n), pre(stones);
        for (int i = 1; i < n; i++)
        {
            for (int j = 0; j < n - i; j++)
            {
                dp[j] = max(pre[j] - dp[j], pre[j + 1] - dp[j + 1]);
                pre[j] += stones[j + i];
            }
        }
        return dp.front();
    }
};
```

__Java__:

```Java
class Solution {
    public int stoneGameVII(int[] stones) {
        int n = stones.length, dp[][] = new int[n][n], pre[] = new int[n + 1];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + stones[i];
        for (int i = n - 2; i > -1; i--) for (int j = i + 1; j < n; j++) dp[i][j] = Math.max(pre[j + 1] - pre[i + 1] - dp[i + 1][j], pre[j] - pre[i] - dp[i][j - 1]);
        return dp[0][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def stoneGameVII(self, stones: List[int]) -> int:
        dp, pre = [0] * (n := len(stones)), stones[:]
        for i in range(1, n):
            for j in range(n - i):
                dp[j] = max(pre[j] - dp[j], pre[j + 1] - dp[j + 1])
                pre[j] += stones[j + i]
        return dp[0]
```
