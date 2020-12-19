__Description__:
A frog is crossing a river. The river is divided into x units and at each unit there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

Given a list of stones' positions (in units) in sorted ascending order, determine if the frog is able to cross the river by landing on the last stone. Initially, the frog is on the first stone and assume the first jump must be 1 unit.

If the frog's last jump was k units, then its next jump must be either k - 1, k, or k + 1 units. Note that the frog can only jump in the forward direction.

__Note:__

The number of stones is ≥ 2 and is < 1,100.
Each stone's position will be a non-negative integer < 2^31.
The first stone's position is always 0.

__Example:__
Example 1:

[0,1,3,5,6,8,12,17]

There are a total of 8 stones.
The first stone at the 0th unit, second stone at the 1st unit,
third stone at the 3rd unit, and so on...
The last stone at the 17th unit.

Return true. The frog can jump to the last stone by jumping 
1 unit to the 2nd stone, then 2 units to the 3rd stone, then 
2 units to the 4th stone, then 3 units to the 6th stone, 
4 units to the 7th stone, and 5 units to the 8th stone.

Example 2:

[0,1,2,3,4,8,9,11]

Return false. There is no way to jump to the last stone as 
the gap between the 5th and 6th stone is too large.

__题目描述__:
一只青蛙想要过河。 假定河流被等分为 x 个单元格，并且在每一个单元格内都有可能放有一石子（也有可能没有）。 青蛙可以跳上石头，但是不可以跳入水中。

给定石子的位置列表（用单元格序号升序表示）， 请判定青蛙能否成功过河（即能否在最后一步跳至最后一个石子上）。 开始时， 青蛙默认已站在第一个石子上，并可以假定它第一步只能跳跃一个单位（即只能从单元格1跳至单元格2）。

如果青蛙上一步跳跃了 k 个单位，那么它接下来的跳跃距离只能选择为 k - 1、k 或 k + 1个单位。 另请注意，青蛙只能向前方（终点的方向）跳跃。

__注意:__

石子的数量 ≥ 2 且 < 1100；
每一个石子的位置序号都是一个非负整数，且其 < 231；
第一个石子的位置永远是0。

__示例 :__
示例 1:

[0,1,3,5,6,8,12,17]

总共有8个石子。
第一个石子处于序号为0的单元格的位置, 第二个石子处于序号为1的单元格的位置,
第三个石子在序号为3的单元格的位置， 以此定义整个数组...
最后一个石子处于序号为17的单元格的位置。

返回 true。即青蛙可以成功过河，按照如下方案跳跃： 
跳1个单位到第2块石子, 然后跳2个单位到第3块石子, 接着 
跳2个单位到第4块石子, 然后跳3个单位到第6块石子, 
跳4个单位到第7块石子, 最后，跳5个单位到第8个石子（即最后一块石子）。

示例 2:

[0,1,2,3,4,8,9,11]

返回 false。青蛙没有办法过河。 
这是因为第5和第6个石子之间的间距太大，没有可选的方案供青蛙跳跃过去。

__思路__:
动态规划
dp[i][j] 表示从 j出发是否能到达 i
dp[i][k] = dp[j][k - 1] or dp[j][k] or dp[j][k + 1], 0 <= j < i, k = stones[j] - stones[i]
k必须不大于 i, 因为从 i = 1开始, 第一次最多跳 1, 以后每次跳的最多为 i + 1
将 dp数组的第二维设置成 n + 1可以防止越界
时间复杂度O(n ^ 2), 空间复杂度O(n ^ 2)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    bool canCross(vector<int>& stones) 
    {
        vector<vector<bool>> dp(stones.size(), vector<bool>(stones.size() + 1));
        dp.front().front() = true;
        for (int i = 1; i < stones.size(); i++)
        {
            for (int j = 0; j < i; j++)
            {
                int k = stones[i] - stones[j];
                if (k < i + 1)
                {
                    dp[i][k] = dp[j][k - 1] or dp[j][k] or dp[j][k + 1];
                    if (i == stones.size() - 1 and dp[i][k]) return true;
                }
            }
        }
        return false;
    }
};
```

__Java__:
```Java
class Solution {
    public boolean canCross(int[] stones) {
        boolean dp[][] = new boolean[stones.length][stones.length + 1];
        dp[0][0] = true;
        for (int i = 1; i < stones.length; i++) {
            for (int j = 0; j < i; j++) {
                int k = stones[i] - stones[j];
                if (k < i + 1) {
                    dp[i][k] = dp[j][k - 1] || dp[j][k] || dp[j][k + 1];
                    if (i == stones.length - 1 && dp[i][k]) return true;
                }
            }
        }
        return false;
    }
}
```

__Python__:
```Python
class Solution:
    def canCross(self, stones: List[int]) -> bool:
        n, dp = len(stones), [[False] * (len(stones) + 1) for _ in range(len(stones))]
        dp[0][0] = True
        for i in range(1, n):
            for j in range(i):
                k = stones[i] - stones[j]
                if k < i + 1:
                    dp[i][k] = dp[j][k] or dp[j][k - 1] or dp[j][k + 1]
                    if i == n - 1 and dp[i][k]:
                        return True
        return False
```