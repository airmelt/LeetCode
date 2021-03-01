# 486 Predict the Winner 预测赢家

__Description__:
Given an array of scores that are non-negative integers. Player 1 picks one of the numbers from either end of the array followed by the player 2 and then player 1 and so on. Each time a player picks a number, that number will not be available for the next player. This continues until all the scores have been chosen. The player with the maximum score wins.

Given an array of scores, predict whether player 1 is the winner. You can assume each player plays to maximize his score.

__Example:__

Example 1:

Input: [1, 5, 2]
Output: False
Explanation: Initially, player 1 can choose between 1 and 2.
If he chooses 2 (or 1), then player 2 can choose from 1 (or 2) and 5. If player 2 chooses 5, then player 1 will be left with 1 (or 2).
So, final score of player 1 is 1 + 2 = 3, and player 2 is 5.
Hence, player 1 will never be the winner and you need to return False.

Example 2:

Input: [1, 5, 233, 7]
Output: True
Explanation: Player 1 first chooses 1. Then player 2 have to choose between 5 and 7. No matter which number player 2 choose, player 1 can choose 233.
Finally, player 1 has more score (234) than player 2 (12), so you need to return True representing player1 can win.

__Constraints:__

1 <= length of the array <= 20.
Any scores in the given array are non-negative integers and will not exceed 10,000,000.
If the scores of both players are equal, then player 1 is still the winner.

__题目描述__:
给定一个表示分数的非负整数数组。 玩家 1 从数组任意一端拿取一个分数，随后玩家 2 继续从剩余数组任意一端拿取分数，然后玩家 1 拿，…… 。每次一个玩家只能拿取一个分数，分数被拿取之后不再可取。直到没有剩余分数可取时游戏结束。最终获得分数总和最多的玩家获胜。

给定一个表示分数的数组，预测玩家1是否会成为赢家。你可以假设每个玩家的玩法都会使他的分数最大化。

__示例 :__

示例 1：

输入：[1, 5, 2]
输出：False
解释：一开始，玩家1可以从1和2中进行选择。
如果他选择 2（或者 1 ），那么玩家 2 可以从 1（或者 2 ）和 5 中进行选择。如果玩家 2 选择了 5 ，那么玩家 1 则只剩下 1（或者 2 ）可选。
所以，玩家 1 的最终分数为 1 + 2 = 3，而玩家 2 为 5 。
因此，玩家 1 永远不会成为赢家，返回 False 。

示例 2：

输入：[1, 5, 233, 7]
输出：True
解释：玩家 1 一开始选择 1 。然后玩家 2 必须从 5 和 7 中进行选择。无论玩家 2 选择了哪个，玩家 1 都可以选择 233 。
     最终，玩家 1（234 分）比玩家 2（12 分）获得更多的分数，所以返回 True，表示玩家 1 可以成为赢家。

__提示:__

1 <= 给定的数组长度 <= 20.
数组里所有分数都为非负数且不会大于 10000000 。
如果最终两个玩家的分数相等，那么玩家 1 仍为赢家。

__思路__:

1. 递归
只用考虑玩家 1的操作是否能赢就行
用一个标记记录是否是玩家 1拿的
从两边分别取数字比较两个玩家拿的数字之和的大小
时间复杂度O(2 ^ n), 空间复杂度O(n)
2. 动态规划
dp[i][j]表示从 nums的区间 [i, j]中玩家 1能取到的最大领先
对于选择 i的情况为 nums[i] - dp[i + 1][j]
对于选择 j的情况为 nums[j] - dp[i][j - 1]
dp[i][j] = max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1])
初始化 dp[i][i] = nums[i]即对角线上的数字对应为 nums中的元素
注意到 dp只由前一行决定, 可以将空间压缩到 O(n)
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool PredictTheWinner(vector<int>& nums) 
    {
        return dfs(0, 0, 0, nums.size() - 1, nums, 1);
    }
private:
    bool dfs(int sum1, int sum2, int l, int r, vector<int>& nums, bool flag)
    {
        if (l > r) return sum1 >= sum2;
        if (flag) return dfs(sum1 + nums[l], sum2, l + 1, r, nums, !flag) or dfs(sum1 + nums[r], sum2, l, r - 1, nums, !flag);
        return dfs(sum1, sum2 + nums[l], l + 1, r, nums, !flag) and dfs(sum1, sum2 + nums[r], l, r - 1, nums, !flag);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean PredictTheWinner(int[] nums) {
        int n = nums.length, dp[][] = new int[nums.length][nums.length];
        for (int i = 0; i < n; i++) dp[i][i] = nums[i];
        for (int k = 1; k < n; k++) for (int i = 0; i < n - k; i++) dp[i][i + k] = Math.max(nums[i] - dp[i + 1][i + k], nums[i + k] - dp[i][i + k - 1]);
        return dp[0][n - 1] >= 0;
    }
}
```

__Python__:

```Python
class Solution:
    def PredictTheWinner(self, nums: List[int]) -> bool:
        n, dp = len(nums), nums[:]
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                dp[j] = max(nums[i] - dp[j], nums[j] - dp[j - 1])
        return dp[-1] >= 0
```
