# 2087 Minimum Cost Homecoming of a Robot in a Grid 网格图中机器人回家的最小代价

__Description:__

There is an `m x n` grid, where `(0, 0)` is the top-left cell and `(m - 1, n - 1)` is the bottom-right cell. You are given an integer array `startPos` where `startPos = [startrow, startcol]` indicates that __initially__, a __robot__ is at the cell `(startrow, startcol)`. You are also given an integer array `homePos` where `homePos = [homerow, homecol]` indicates that its __home__ is at the cell `(homerow, homecol)`.

The robot needs to go to its home. It can move one cell in four directions: __left__, __right__, __up__, or __down__, and it can not move outside the boundary. Every move incurs some cost. You are further given two __0-indexed__ integer arrays: `rowCosts` of length `m` and `colCosts` of length `n`.

- If the robot moves __up__ or __down__ into a cell whose __row__ is `r`, then this move costs `rowCosts[r]`.
- If the robot moves __left__ or __right__ into a cell whose __column__ is `c`, then this move costs `colCosts[c]`.

Return the minimum total cost for this robot to return home.

__Example:__

Example 1:

![2087-1](https://assets.leetcode.com/uploads/2021/10/11/eg-1.png)

```text
Input: startPos = [1, 0], homePos = [2, 3], rowCosts = [5, 4, 3], colCosts = [8, 2, 6, 7]
Output: 18
Explanation: One optimal path is that:
Starting from (1, 0)
-> It goes down to (2, 0). This move costs rowCosts[2] = 3.
-> It goes right to (2, 1). This move costs colCosts[1] = 2.
-> It goes right to (2, 2). This move costs colCosts[2] = 6.
-> It goes right to (2, 3). This move costs colCosts[3] = 7.
The total cost is 3 + 2 + 6 + 7 = 18
```

Example 2:

```text
Input: startPos = [0, 0], homePos = [0, 0], rowCosts = [5], colCosts = [26]
Output: 0
Explanation: The robot is already at its home. Since no moves occur, the total cost is 0.
```

__Constraints:__

- `m == rowCosts.length`
- `n == colCosts.length`
- `1 <= m, n <= 10 ^ 5`
- `0 <= rowCosts[r], colCosts[c] <= 10 ^ 4`
- `startPos.length == 2`
- `homePos.length == 2`
- `0 <= startrow, homerow < m`
- `0 <= startcol, homecol < n`

__题目描述:__

给你一个 `m x n` 的网格图，其中 `(0, 0)` 是最左上角的格子， `(m - 1, n - 1)` 是最右下角的格子。给你一个整数数组 `startPos` ， `startPos = [startrow, startcol]` 表示 __初始__ 有一个 __机器人__ 在格子 `(startrow, startcol)` 处。同时给你一个整数数组 `homePos` ， `homePos = [homerow, homecol]` 表示机器人的 __家__ 在格子 `(homerow, homecol)` 处。

机器人需要回家。每一步它可以往四个方向移动:__上__，__下__，__左__，__右__，同时机器人不能移出边界。每一步移动都有一定代价。再给你两个下标从 __0__ 开始的额整数数组:长度为 `m` 的数组 `rowCosts` 和长度为 `n` 的数组 `colCosts` 。

- 如果机器人往 __上__ 或者往 __下__ 移动到第 `r` __行__ 的格子，那么代价为 `rowCosts[r]` 。
- 如果机器人往 __左__ 或者往 __右__ 移动到第 `c` __列__ 的格子，那么代价为 `colCosts[c]` 。

请你返回机器人回家需要的 最小总代价 。

__示例:__

示例 1：

![2087-2](https://assets.leetcode.com/uploads/2021/10/11/eg-1.png)

```text
输入：startPos = [1, 0], homePos = [2, 3], rowCosts = [5, 4, 3], colCosts = [8, 2, 6, 7]
输出：18
解释：一个最优路径为：
从 (1, 0) 开始
-> 往下走到 (2, 0) 。代价为 rowCosts[2] = 3 。
-> 往右走到 (2, 1) 。代价为 colCosts[1] = 2 。
-> 往右走到 (2, 2) 。代价为 colCosts[2] = 6 。
-> 往右走到 (2, 3) 。代价为 colCosts[3] = 7 。
总代价为 3 + 2 + 6 + 7 = 18
```

示例 2：

```text
输入：startPos = [0, 0], homePos = [0, 0], rowCosts = [5], colCosts = [26]
输出：0
解释：机器人已经在家了，所以不需要移动。总代价为 0 。
```

__提示：__

- `m == rowCosts.length`
- `n == colCosts.length`
- `1 <= m, n <= 10 ^ 5`
- `0 <= rowCosts[r], colCosts[c] <= 10 ^ 4`
- `startPos.length == 2`
- `homePos.length == 2`
- `0 <= startrow, homerow < m`
- `0 <= startcol, homecol < n`

__思路:__

```text
贪心
直接从起点沿最短路径一定是最小代价
起点到终点的横坐标范围为 [min(startPos[0], homePos[0]), max(startPos[0], homePos[0])]
将这个范围内的 rowCosts 求和即可
起点到终点的纵坐标范围为 [min(startPos[1], homePos[1]), max(startPos[1], homePos[1])]
将这个范围内的 colCosts 求和即可
最后由于起点不需要计算, 所以减去起点的代价即可
时间复杂度为 O(N + M), 空间复杂度为 O(1), 其中 N 为 rowCosts 数组的长度, M 为 colCosts 数组的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minCost(vector<int>& startPos, vector<int>& homePos, vector<int>& rowCosts, vector<int>& colCosts) 
    {
        return accumulate(rowCosts.begin() + min(startPos.front(), homePos.front()), rowCosts.begin() + max(startPos.front(), homePos.front()) + 1, 0) + accumulate(colCosts.begin() + min(startPos.back(), homePos.back()), colCosts.begin() + max(startPos.back(), homePos.back()) + 1, 0) - rowCosts[startPos.front()] - colCosts[startPos.back()];
    }
};
```

__Java__:

```Java
class Solution {
    public int minCost(int[] startPos, int[] homePos, int[] rowCosts, int[] colCosts) {
        return IntStream.of(Arrays.copyOfRange(rowCosts, Math.min(startPos[0], homePos[0]), Math.max(startPos[0], homePos[0]) + 1)).sum() + IntStream.of(Arrays.copyOfRange(colCosts, Math.min(startPos[1], homePos[1]), Math.max(startPos[1], homePos[1]) + 1)).sum() - rowCosts[startPos[0]] - colCosts[startPos[1]];
    }
}
```

__Python__:

```Python
class Solution:
    def minCost(self, startPos: List[int], homePos: List[int], rowCosts: List[int], colCosts: List[int]) -> int:
        return (sum(rowCosts[i] for i in range(r1 + 1, r2 + 1)) if (r2 := homePos[0]) >= (r1 := startPos[0]) else sum(rowCosts[i] for i in range(r2, r1))) + (sum(colCosts[i] for i in range(c1 + 1, c2 + 1)) if (c2 := homePos[1]) >= (c1 := startPos[1]) else sum(colCosts[i] for i in range(c2, c1)))
```
