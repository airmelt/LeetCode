# 789 Escape The Ghosts 逃脱阻碍者

__Description__:
You are playing a simplified PAC-MAN game on an infinite 2-D grid. You start at the point [0, 0], and you are given a destination point target = [xtarget, ytarget], which you are trying to get to. There are several ghosts on the map with their starting positions given as an array ghosts, where ghosts[i] = [xi, yi] represents the starting position of the ith ghost. All inputs are integral coordinates.

Each turn, you and all the ghosts may independently choose to either move 1 unit in any of the four cardinal directions: north, east, south, or west or stay still. All actions happen simultaneously.

You escape if and only if you can reach the target before any ghost reaches you. If you reach any square (including the target) at the same time as a ghost, it does not count as an escape.

Return true if it is possible to escape, otherwise return false.

__Example:__

Example 1:

Input: ghosts = [[1,0],[0,3]], target = [0,1]
Output: true
Explanation: You can reach the destination (0, 1) after 1 turn, while the ghosts located at (1, 0) and (0, 3) cannot catch up with you.

Example 2:

Input: ghosts = [[1,0]], target = [2,0]
Output: false
Explanation: You need to reach the destination (2, 0), but the ghost at (1, 0) lies between you and the destination.

Example 3:

Input: ghosts = [[2,0]], target = [1,0]
Output: false
Explanation: The ghost can reach the target at the same time as you.

Example 4:

Input: ghosts = [[5,0],[-10,-2],[0,-5],[-2,-2],[-7,1]], target = [7,7]
Output: false

Example 5:

Input: ghosts = [[-1,0],[0,1],[-1,0],[0,1],[-1,0]], target = [0,0]
Output: true

__Constraints:__

1 <= ghosts.length <= 100
ghosts[i].length == 2
-10^4 <= xi, yi <= 10^4
There can be multiple ghosts in the same location.
target.length == 2
-10^4 <= xtarget, ytarget <= 10^4

__题目描述__:
你在进行一个简化版的吃豆人游戏。你从 [0, 0] 点开始出发，你的目的地是 target = [xtarget, ytarget] 。地图上有一些阻碍者，以数组 ghosts 给出，第 i 个阻碍者从 ghosts[i] = [xi, yi] 出发。所有输入均为 整数坐标 。

每一回合，你和阻碍者们可以同时向东，西，南，北四个方向移动，每次可以移动到距离原位置 1 个单位 的新位置。当然，也可以选择 不动 。所有动作 同时 发生。

如果你可以在任何阻碍者抓住你 之前 到达目的地（阻碍者可以采取任意行动方式），则被视为逃脱成功。如果你和阻碍者同时到达了一个位置（包括目的地）都不算是逃脱成功。

只有在你有可能成功逃脱时，输出 true ；否则，输出 false 。

__示例 :__

示例 1：

输入：ghosts = [[1,0],[0,3]], target = [0,1]
输出：true
解释：你可以直接一步到达目的地 (0,1) ，在 (1, 0) 或者 (0, 3) 位置的阻碍者都不可能抓住你。

示例 2：

输入：ghosts = [[1,0]], target = [2,0]
输出：false
解释：你需要走到位于 (2, 0) 的目的地，但是在 (1, 0) 的阻碍者位于你和目的地之间。

示例 3：

输入：ghosts = [[2,0]], target = [1,0]
输出：false
解释：阻碍者可以和你同时达到目的地。

示例 4：

输入：ghosts = [[5,0],[-10,-2],[0,-5],[-2,-2],[-7,1]], target = [7,7]
输出：false

示例 5：

输入：ghosts = [[-1,0],[0,1],[-1,0],[0,1],[-1,0]], target = [0,0]
输出：true

__提示:__

1 <= ghosts.length <= 100
ghosts[i].length == 2
-10^4 <= xi, yi <= 10^4
同一位置可能有 多个阻碍者 。
target.length == 2
-10^4 <= xtarget, ytarget <= 10^4

__思路__:

曼哈顿距离
只要 ghost 能在玩家之前到达 target 就在 target 等着就行
所以只用计算 ghost 和 target 的曼哈顿距离 |x - target[0]| + |y - target[1]| 和玩家与 target 的曼哈顿距离, 只有玩家的曼哈顿距离更小才能到达 target
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool escapeGhosts(vector<vector<int>>& ghosts, vector<int>& target) 
    {
        for (const auto& ghost : ghosts) if (abs(ghost.front() - target.front()) + abs(ghost.back() - target.back()) <= abs(target.front()) + abs(target.back())) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean escapeGhosts(int[][] ghosts, int[] target) {
        for (int[] ghost : ghosts) if (Math.abs(ghost[0] - target[0]) + Math.abs(ghost[1] - target[1]) <= Math.abs(target[0]) + Math.abs(target[1])) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def escapeGhosts(self, ghosts: List[List[int]], target: List[int]) -> bool:
        return all(abs(x - (a := target[0])) + abs(y - (b := target[1])) > (s := (abs(a) + abs(b))) for x, y in ghosts)
```
