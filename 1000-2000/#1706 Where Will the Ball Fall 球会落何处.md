# 1706 Where Will the Ball Fall 球会落何处

__Description:__

You have a 2-D `grid` of size `m x n` representing a box, and you have `n` balls. The box is open on the top and bottom sides.

Each cell in the box has a diagonal board spanning two corners of the cell that can redirect a ball to the right or to the left.

- A board that redirects the ball to the right spans the top-left corner to the bottom-right corner and is represented in the grid as `1`.
- A board that redirects the ball to the left spans the top-right corner to the bottom-left corner and is represented in the grid as `-1`.

We drop one ball at the top of each column of the box. Each ball can get stuck in the box or fall out of the bottom. A ball gets stuck if it hits a "V" shaped pattern between two boards or if a board redirects the ball into either wall of the box.

Return _an array_ `answer` _of size_ `n` _where_ `answer[i]` _is the column that the ball falls out of at the bottom after dropping the ball from the_ `i ^ th` _column at the top, or `-1`_ if the ball gets stuck in the box _._

__Example:__

Example 1:

![1706-1](https://assets.leetcode.com/uploads/2019/09/26/ball.jpg)

```text
Input: grid = [[1,1,1,-1,-1],[1,1,1,-1,-1],[-1,-1,-1,1,1],[1,1,1,1,-1],[-1,-1,-1,-1,-1]]
Output: [1,-1,-1,-1,-1]
Explanation: This example is shown in the photo.
Ball b0 is dropped at column 0 and falls out of the box at column 1.
Ball b1 is dropped at column 1 and will get stuck in the box between column 2 and 3 and row 1.
Ball b2 is dropped at column 2 and will get stuck on the box between column 2 and 3 and row 0.
Ball b3 is dropped at column 3 and will get stuck on the box between column 2 and 3 and row 0.
Ball b4 is dropped at column 4 and will get stuck on the box between column 2 and 3 and row 1.
```

Example 2:

```text
Input: grid = [[-1]]
Output: [-1]
Explanation: The ball gets stuck against the left wall.
```

Example 3:

```text
Input: grid = [[1,1,1,1,1,1],[-1,-1,-1,-1,-1,-1],[1,1,1,1,1,1],[-1,-1,-1,-1,-1,-1]]
Output: [0,1,2,3,4,-1]
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100`
- `grid[i][j]` is `1` or `-1`.

__题目描述:__

用一个大小为 `m x n` 的二维网格 `grid` 表示一个箱子。你有 `n` 颗球。箱子的顶部和底部都是开着的。

箱子中的每个单元格都有一个对角线挡板，跨过单元格的两个角，可以将球导向左侧或者右侧。

- 将球导向右侧的挡板跨过左上角和右下角，在网格中用 `1` 表示。
- 将球导向左侧的挡板跨过右上角和左下角，在网格中用 `-1` 表示。

在箱子每一列的顶端各放一颗球。每颗球都可能卡在箱子里或从底部掉出来。如果球恰好卡在两块挡板之间的 "V" 形图案，或者被一块挡导向到箱子的任意一侧边上，就会卡住。

返回一个大小为 `n` 的数组 `answer` ，其中 `answer[i]` 是球放在顶部的第 `i` 列后从底部掉出来的那一列对应的下标，如果球卡在盒子里，则返回 `-1` 。

__示例:__

示例 1：

![1706-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/12/26/ball.jpg)

输入：grid = [[1,1,1,-1,-1],[1,1,1,-1,-1],[-1,-1,-1,1,1],[1,1,1,1,-1],[-1,-1,-1,-1,-1]]
输出：[1,-1,-1,-1,-1]
解释：示例如图：
b0 球开始放在第 0 列上，最终从箱子底部第 1 列掉出。
b1 球开始放在第 1 列上，会卡在第 2、3 列和第 1 行之间的 "V" 形里。
b2 球开始放在第 2 列上，会卡在第 2、3 列和第 0 行之间的 "V" 形里。
b3 球开始放在第 3 列上，会卡在第 2、3 列和第 0 行之间的 "V" 形里。
b4 球开始放在第 4 列上，会卡在第 2、3 列和第 1 行之间的 "V" 形里。

示例 2：

```text
输入：grid = [[-1]]
输出：[-1]
解释：球被卡在箱子左侧边上。
```

示例 3：

```text
输入：grid = [[1,1,1,1,1,1],[-1,-1,-1,-1,-1,-1],[1,1,1,1,1,1],[-1,-1,-1,-1,-1,-1]]
输出：[0,1,2,3,4,-1]
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100`
- `grid[i][j]` 为 `1` 或 `-1`

__思路:__

```text
模拟
模拟每一个小球的运动轨迹, 小球卡住提前终止
每到新的一行, 分 -1 和 1 两种情况讨论小球的运动方向
时间复杂度为 O(MN), 空间复杂度为 O(1), 不计返回的结果空间复杂度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findBall(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size();
        vector<int> result(n);
        for (int i = 0; i < n; i++) 
        {
            int count = i;
            for (int j = 0; j < m; j++) 
            {
                if (grid[j][count] == -1) 
                {
                    if (count > 0 and grid[j][count - 1] == -1) --count;
                    else 
                    {
                        count = -1;
                        break;
                    }
                }
                else 
                {
                    if (count < n - 1 and grid[j][count + 1] == 1) ++count;
                    else 
                    {
                        count = -1;
                        break;
                    }
                }
            }
            result[i] = count;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findBall(int[][] grid) {
        int m = grid.length, n = grid[0].length, result[] = new int[n];
        for (int i = 0; i < n; i++) {
            int count = i;
            for (int j = 0; j < grid.length; j++) {
                if (grid[j][count] == -1) {
                    if (count > 0 && grid[j][count - 1] == -1) --count;
                    else {
                        count = -1;
                        break;
                    }
                }
                else {
                    if (count < n - 1 && grid[j][count + 1] == 1) ++count;
                    else {
                        count = -1;
                        break;
                    }
                }
            }
            result[i] = count;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findBall(self, grid: List[List[int]]) -> List[int]:
        m, result = len(grid), [0] * (n := len(grid[0]))
        for i in range(n):
            count = i
            for j in range(m):
                if grid[j][count] == -1:
                    if count > 0 and grid[j][count - 1] == -1:
                        count -= 1
                    else:
                        count = -1
                        break
                else:
                    if count < n - 1 and grid[j][count + 1] == 1:
                        count += 1
                    else:
                        count = -1
                        break
            result[i] = count
        return result
```
