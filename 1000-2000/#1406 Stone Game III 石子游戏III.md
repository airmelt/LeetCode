# 1406 Stone Game III 石子游戏III

__Description:__

Alice and Bob continue their games with piles of stones. There are several stones __arranged in a row__, and each stone has an associated value which is an integer given in the array `stoneValue`.

Alice and Bob take turns, with Alice starting first. On each player's turn, that player can take `1`, `2`, or `3` stones from the __first__ remaining stones in the row.

The score of each player is the sum of the values of the stones taken. The score of each player is `0` initially.

The objective of the game is to end with the highest score, and the winner is the player with the highest score and there could be a tie. The game continues until all the stones have been taken.

Assume Alice and Bob play optimally.

Return `"Alice"` _if Alice will win,_ `"Bob"` _if Bob will win, or_ `"Tie"` _if they will end the game with the same score_.

__Example:__

Example 1:

```text
Input: values = [1,2,3,7]
Output: "Bob"
Explanation: Alice will always lose. Her best move will be to take three piles and the score become 6. Now the score of Bob is 7 and Bob wins.
```

Example 2:

```text
Input: values = [1,2,3,-9]
Output: "Alice"
Explanation: Alice must choose all the three piles at the first move to win and leave Bob with negative score.
If Alice chooses one pile her score will be 1 and the next move Bob's score becomes 5. In the next move, Alice will take the pile with value = -9 and lose.
If Alice chooses two piles her score will be 3 and the next move Bob's score becomes 3. In the next move, Alice will take the pile with value = -9 and also lose.
Remember that both play optimally so here Alice will choose the scenario that makes her win.
```

Example 3:

```text
Input: values = [1,2,3,6]
Output: "Tie"
Explanation: Alice cannot win this game. She can end the game in a draw if she decided to choose all the first three piles, otherwise she will lose.
```

__Constraints:__

- 1 <= stoneValue.length <= 5 * 10 ^ 4
- -1000 <= stoneValue[i] <= 1000

__题目描述:__

Alice 和 Bob 用几堆石子在做游戏。几堆石子排成一行，每堆石子都对应一个得分，由数组 `stoneValue` 给出。

Alice 和 Bob 轮流取石子，Alice 总是先开始。在每个玩家的回合中，该玩家可以拿走剩下石子中的的前 1、2 或 3 堆石子 。比赛一直持续到所有石头都被拿走。

每个玩家的最终得分为他所拿到的每堆石子的对应得分之和。每个玩家的初始分数都是 0 。比赛的目标是决出最高分，得分最高的选手将会赢得比赛，比赛也可能会出现平局。

假设 Alice 和 Bob 都采取 最优策略 。如果 Alice 赢了就返回 "Alice" ，Bob 赢了就返回 "Bob"，平局（分数相同）返回 "Tie" 。

__示例:__

示例 1：

```text
输入：values = [1,2,3,7]
输出："Bob"
解释：Alice 总是会输，她的最佳选择是拿走前三堆，得分变成 6 。但是 Bob 的得分为 7，Bob 获胜。
```

示例 2：

```text
输入：values = [1,2,3,-9]
输出："Alice"
解释：Alice 要想获胜就必须在第一个回合拿走前三堆石子，给 Bob 留下负分。
如果 Alice 只拿走第一堆，那么她的得分为 1，接下来 Bob 拿走第二、三堆，得分为 5 。之后 Alice 只能拿到分数 -9 的石子堆，输掉比赛。
如果 Alice 拿走前两堆，那么她的得分为 3，接下来 Bob 拿走第三堆，得分为 3 。之后 Alice 只能拿到分数 -9 的石子堆，同样会输掉比赛。
注意，他们都应该采取 最优策略 ，所以在这里 Alice 将选择能够使她获胜的方案。
```

示例 3：

```text
输入：values = [1,2,3,6]
输出："Tie"
解释：Alice 无法赢得比赛。如果她决定选择前三堆，她可以以平局结束比赛，否则她就会输。
```

示例 4：

```text
输入：values = [1,2,3,-1,-2,-3,7]
输出："Alice"
```

示例 5：

```text
输入：values = [-1,-2,-3]
输出："Tie"
```

__提示：__

- 1 <= values.length <= 50000
- -1000 <= values[i] <= 1000

__思路:__

```text
动态规划
博弈类型的动态规划
假设 dp[i] 表示当获取完前 i 堆石头能比对方多的分数
现在准备获取 i, i + 1, i + 2, ... 的石头
如果拿 i 堆, 那么对方最多可获取 i + 1, i + 2, ... 的石头
dp[i] = stoneValue[i] - dp[i + 1]
如果拿 i, i + 1 堆以及 i, i + 1, i + 2 情况类似
那么 dp[i] 可以取上述三种情况最大值
为了方便计算增加 3 堆空石子在最后
因为计算 dp[i] 需要使用 dp[i + 1], dp[i + 2], dp[i + 3]
所以需要从后往前遍历更新 dp 数组
最后比较 dp[0] 和 0 的大小关系决定胜负
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    string stoneGameIII(vector<int>& stoneValue) 
    {
        int n = stoneValue.size();
        vector<int> dp(n + 3, 0);
        stoneValue.insert(stoneValue.end(), 3, 0);
        for (int i = n - 1; i >= 0; i--) dp[i] = max({ stoneValue[i] - dp[i + 1], stoneValue[i] + stoneValue[i + 1] - dp[i + 2], stoneValue[i] + stoneValue[i + 1] + stoneValue[i + 2] - dp[i + 3] });
        return !dp.front() ? "Tie" : dp.front() > 0 ? "Alice" : "Bob";
    }
};
```

__Java__:

```Java
class Solution {
    public String stoneGameIII(int[] stoneValue) {
        int n = stoneValue.length, stones[] = new int[n + 3], dp[] = new int[n + 3];
        for (int i = 0; i < n; i++) stones[i] = stoneValue[i];
        for (int i = n - 1; i > -1; i--) dp[i] = Math.max(stones[i] - dp[i + 1], Math.max(stones[i] + stones[i + 1] - dp[i + 2], stones[i] + stones[i + 1] + stones[i + 2] - dp[i + 3]));
        return dp[0] == 0 ? "Tie" : dp[0] > 0 ? "Alice" : "Bob";
    }
}
```

__Python__:

```Python
class Solution:
    def stoneGameIII(self, stoneValue: List[int]) -> str:
        dp, stones = [0] * ((n := len(stoneValue)) + 3), list(accumulate([0] + stoneValue + [0] * 3))
        for i in range(n - 1, -1, -1):
            dp[i] = max(stones[i + 1] - stones[i] - dp[i + 1], stones[i + 2] - stones[i] - dp[i + 2], stones[i + 3] - stones[i] - dp[i + 3])
        return "Tie" if not dp[0] else "Alice" if dp[0] > 0 else "Bob"
```
