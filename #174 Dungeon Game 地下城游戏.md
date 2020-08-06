__Description__:
The demons had captured the princess (P) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid. Our valiant knight (K) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.

Some of the rooms are guarded by demons, so the knight loses health (negative integers) upon entering these rooms; other rooms are either empty (0's) or contain magic orbs that increase the knight's health (positive integers).

In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.

Write a function to determine the knight's minimum initial health so that he is able to rescue the princess.

__Example:__
For example, given the dungeon below, the initial health of the knight must be at least 7 if he follows the optimal path RIGHT-> RIGHT -> DOWN -> DOWN.

| | | |
| :---- | :---- | :---- |
|-2 (K)	|-3	|3|
|-5	|-10	|1|
|10	|30	|-5 (P)|


__Note:__

The knight's health has no upper bound.
Any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.

__题目描述__:
一些恶魔抓住了公主（P）并将她关在了地下城的右下角。地下城是由 M x N 个房间组成的二维网格。我们英勇的骑士（K）最初被安置在左上角的房间里，他必须穿过地下城并通过对抗恶魔来拯救公主。

骑士的初始健康点数为一个正整数。如果他的健康点数在某一时刻降至 0 或以下，他会立即死亡。

有些房间由恶魔守卫，因此骑士在进入这些房间时会失去健康点数（若房间里的值为负整数，则表示骑士将损失健康点数）；其他房间要么是空的（房间里的值为 0），要么包含增加骑士健康点数的魔法球（若房间里的值为正整数，则表示骑士将增加健康点数）。

为了尽快到达公主，骑士决定每次只向右或向下移动一步。

编写一个函数来计算确保骑士能够拯救到公主所需的最低初始健康点数。

__示例 :__
例如，考虑到如下布局的地下城，如果骑士遵循最佳路径 右 -> 右 -> 下 -> 下，则骑士的初始健康点数至少为 7。

| | | |
| :---- | :---- | :---- |
|-2 (K)	|-3	|3|
|-5	|-10	|1|
|10	|30	|-5 (P)|
 
__说明：__

骑士的健康点数没有上限。

任何房间都可能对骑士的健康点数造成威胁，也可能增加骑士的健康点数，包括骑士进入的左上角房间以及公主被监禁的右下角房间。

__思路__:
参考[LeetCode #64 Minimum Path Sum 最小路径和](https://www.jianshu.com/p/b1ca6a1445a6)
动态规划
因为数组的值受到后面的状态影响, 应该从后往前遍历, 找最小值
要求的最小生命值为 1
状态转移方程:
dp[i][j] = max(1, min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j])
初始状态, 即到公主的点, 至少要有 1点生命
dp.back().back() = max(1, 1 - dungeon.back().back())
时间复杂度O(mn), 空间复杂度O(m), 使用滚动数组可以优化时间复杂度

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int calculateMinimumHP(vector<vector<int>>& dungeon) 
    {
        int dp[dungeon[0].size() + 1], row = dungeon.size(), col = dungeon[0].size();
        for (int j = 0; j <= col; j++) dp[j] = INT_MAX;
        for (int i = row - 1; ~i; i--)
        {
            dp[row] = (i == row - 1) ? 1 : INT_MAX;
            for (int j = col - 1; ~j; j--) dp[j] = max(min(dp[j + 1], dp[j]) - dungeon[i][j], 1);
        }
        return dp[0];
    }
};
```

__Java__:
```Java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        int dp[][] = new int[dungeon.length][dungeon[0].length], row = dungeon.length, col = dungeon[0].length;
        for (int i = 0; i < row; i++) for (int j = 0; j < col; j++) dp[i][j] = 1;
        for (int i = row - 1; i > -1; i--) {
            for (int j = col - 1; j > -1; j--) {
                if (i == row - 1 && j == col - 1) dp[i][j] = Math.max(1, dp[i][j] - dungeon[i][j]);
                else if (j == col - 1) dp[i][j] = Math.max(1, dp[i + 1][j] - dungeon[i][j]);
                else if (i == row - 1) dp[i][j] = Math.max(1, dp[i][j + 1] - dungeon[i][j]);
                else dp[i][j] = Math.max(1, Math.min(dp[i][j + 1], dp[i + 1][j]) - dungeon[i][j]);
            }
        }
        return dp[0][0];
    }
}
```

__Python__:
```Python
class Solution:
    def calculateMinimumHP(self, dungeon: List[List[int]]) -> int:
        row, col, dp = len(dungeon), len(dungeon[0]), [[1] * len(dungeon[0]) for _ in range(len(dungeon))]
        for i in range(row - 1, -1, -1):
            for j in range(col - 1, -1, -1):
                if i == row - 1 and j == col - 1:
                    dp[i][j] = max(1, 1 - dungeon[i][j])
                elif i == row - 1:
                    dp[i][j] = max(1, dp[i][j + 1] - dungeon[i][j])
                elif j == col - 1:
                    dp[i][j] = max(1, dp[i + 1][j] - dungeon[i][j])
                else:
                    dp[i][j] = max(1, min(dp[i + 1][j] - dungeon[i][j], dp[i][j + 1] - dungeon[i][j]))
        return dp[0][0]
```