# 3197 Find the Minimum Area to Cover All Ones II 包含所有 1 的最小矩形面积 II

__Description:__

You are given a 2D __binary__ array `grid`. You need to find 3 __non-overlapping__ rectangles having __non-zero__ areas with horizontal and vertical sides such that all the 1's in `grid` lie inside these rectangles.

Return the __minimum__ possible sum of the area of these rectangles.

__Note__ that the rectangles are allowed to touch.

__Example:__

Example 1:

```text
Input: grid = [[1,0,1],[1,1,1]]

Output: 5

Explanation:
```

![3197-1](https://assets.leetcode.com/uploads/2024/05/14/example0rect21.png)

- The 1's at `(0, 0)` and `(1, 0)` are covered by a rectangle of area 2.
- The 1's at `(0, 2)` and `(1, 2)` are covered by a rectangle of area 2.
- The 1 at `(1, 1)` is covered by a rectangle of area 1.

Example 2:

```text
Input: grid = [[1,0,1,0],[0,1,0,1]]

Output: 5

Explanation:
```

![3197-2](https://assets.leetcode.com/uploads/2024/05/14/example1rect2.png)

- The 1's at `(0, 0)` and `(0, 2)` are covered by a rectangle of area 3.
- The 1 at `(1, 1)` is covered by a rectangle of area 1.
- The 1 at `(1, 3)` is covered by a rectangle of area 1.

__Constraints:__

- `1 <= grid.length, grid[i].length <= 30`
- `grid[i][j]` is either 0 or 1.
- The input is generated such that there is at least one 1 in `grid`.

__题目描述:__

给你一个二维 __二进制__ 数组 `grid`。你需要找到 3 个 __不重叠__、面积 __非零__ 、边在水平方向和竖直方向上的矩形，并且满足 `grid` 中所有的 1 都在这些矩形的内部。

返回这些矩形面积之和的 __最小__ 可能值。

__注意__，这些矩形可以相接。

__示例:__

示例 1：

```text
输入： grid = [[1,0,1],[1,1,1]]

输出： 5

解释：
```

![3197-3](https://assets.leetcode.com/uploads/2024/05/14/example0rect21.png)

- 位于 `(0, 0)` 和 `(1, 0)` 的 1 被一个面积为 2 的矩形覆盖。
- 位于 `(0, 2)` 和 `(1, 2)` 的 1 被一个面积为 2 的矩形覆盖。
- 位于 `(1, 1)` 的 1 被一个面积为 1 的矩形覆盖。

示例 2：

```text
输入： grid = [[1,0,1,0],[0,1,0,1]]

输出： 5

解释：
```

![3197-4](https://assets.leetcode.com/uploads/2024/05/14/example1rect2.png)

- 位于 `(0, 0)` 和 `(0, 2)` 的 1 被一个面积为 3 的矩形覆盖。
- 位于 `(1, 1)` 的 1 被一个面积为 1 的矩形覆盖。
- 位于 `(1, 3)` 的 1 被一个面积为 1 的矩形覆盖。

__提示：__

- `1 <= grid.length, grid[i].length <= 30`
- `grid[i][j]` 是 0 或 1。
- 输入保证 `grid` 中至少有一个 1 。

__思路:__

```text
动态规划
设 dp[i + 1][j + 1] 表示左上角为 (0, 0), 右下角为 (i, j) 包含里面所有的 1 的最小矩形
border[i + 1][j + 1] 分别表示上述最小矩形的上边界, 左边界和右边界
left 为 1 出现的最左边的列, 初始值为 -1, right 为 1 出现的最右边的列
如果当前行全 0, 那么 dp[i + 1][j + 1] = dp[i][j + 1], border[i + 1][j + 1] = border[i][j + 1]
如果当前行有 1, 且前 i 行均为 0, dp[i + 1][j + 1] = right - left + 1, border[i + 1][j + 1] = (i, left, right)
否则
上边界 t 为 border[i][j + 1][0]
左边界 l 为 min(left, border[i][j + 1][1])
右边界 r 为 max(right, border[i][j + 1][2])
下边界为 i
dp[i + 1][j + 1] = (r - l + 1) * (i - t + 1)
border[i + 1][j + 1] = (t, l, r)
旋转得到 4 个方向的矩形
最后两种情况
记录 1 出现的边界枚举即可
时间复杂度为 O(NM), 空间复杂度为 O(NM)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumSum(vector<vector<int>>& grid) 
    {
        auto a = rotate(grid);
        return min(helper(grid), helper(a));
    }
private:
    vector<vector<int>> rotate(const vector<vector<int>> a) 
    {
        int m = a.size(), n = a.front().size();
        vector result(n, vector<int>(m));
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result[j][m - 1 - i] = a[i][j];
        return result;
    }

    vector<vector<int>> minimumArea(const vector<vector<int>>& a) 
    {
        int m = a.size(), n = a.front().size();
        vector dp(m + 1, vector<int>(n + 1));
        vector<tuple<int, int, int>> border(n + 1, {-1, -1, -1});
        for (int i = 0, left = -1, right = 0, r = 0, l = 0; i < m; i++, left = -1, right = 0) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (a[i][j] == 1) 
                {
                    left = left < 0 ? j : left;
                    right = j;
                }
                auto& [pre_top, pre_left, pre_right] = border[j];
                if (left < 0) dp[i + 1][j + 1] = dp[i][j + 1];
                else if (pre_top < 0) 
                {
                    dp[i + 1][j + 1] = right - left + 1;
                    border[j] = {i, left, right};
                } 
                else 
                {
                    dp[i + 1][j + 1] = ((r = max(pre_right, right)) - (l = min(pre_left, left)) + 1) * (i - pre_top + 1);
                    border[j] = {pre_top, l, r};
                }
            }
        }
        return dp;
    }

    int helper(vector<vector<int>>& a) 
    {
        int m = a.size(), n = a.front().size(), result = INT_MAX;
        vector<pair<int, int>> lr(m);
        for (int i = 0, l = -1, r = 0; i < m; i++, l = -1, r = 0) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (a[i][j] > 0) 
                {
                    l = l < 0 ? j : l;
                    r = j;
                }
            }
            lr[i] = { l, r };
        }
        vector<vector<int>> lt = minimumArea(a);
        a = rotate(a);
        vector<vector<int>> lb = rotate(rotate(rotate(minimumArea(a))));
        a = rotate(a);
        vector<vector<int>> rb = rotate(rotate(minimumArea(a)));
        a = rotate(a);
        vector<vector<int>> rt = rotate(minimumArea(a));
        if (m >= 3) 
        {
            for (int i = 1, left = n, right = -1, top = m, bottom = -1; i < m; i++, left = n, right = -1, top = m, bottom = -1) 
            {
                for (int j = i + 1, l = -1; j < m; j++) 
                {
                    if ((l = lr[j - 1].first) > -1) 
                    {
                        left = min(left, l);
                        right = max(right, lr[j - 1].second);
                        top = min(top, j - 1);
                        bottom = j - 1;
                    }
                    result = min(result, lt[i][n] + (right - left + 1) * (bottom - top + 1) + lb[j][n]);
                }
            }
        }
        if (m >= 2 and n >= 2) for (int i = 1; i < m; i++) for (int j = 1; j < n; j++) result = min({ result, lt[i][n] + lb[i][j] + rb[i][j], lt[i][j] + rt[i][j] + lb[i][n] });
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumSum(int[][] grid) {
        return Math.min(helper(grid), helper(rotate(grid)));
    }

    private int helper(int[][] a) {
        int m = a.length, n = a[0].length, lr[][] = new int[m][2], result = Integer.MAX_VALUE;
        for (int i = 0, l = -1, r = 0; i < m; i++, l = -1, r = 0) {
            for (int j = 0; j < n; j++) {
                if (a[i][j] > 0) {
                    l = l < 0 ? j : l;
                    r = j;
                }
            }
            lr[i][0] = l;
            lr[i][1] = r;
        }

        int[][] lt = minimumArea(a);
        a = rotate(a);
        int[][] lb = rotate(rotate(rotate(minimumArea(a))));
        a = rotate(a);
        int[][] rb = rotate(rotate(minimumArea(a)));
        a = rotate(a);
        int[][] rt = rotate(minimumArea(a));
        if (m >= 3) {
            for (int i = 1, left = n, right = -1, top = m, bottom = -1; i < m; i++, left = n, right = -1, top = m, bottom = -1) {
                for (int j = i + 1, l = -1; j < m; j++) {
                    if ((l = lr[j - 1][0]) > -1) {
                        left = Math.min(left, l);
                        right = Math.max(right, lr[j - 1][1]);
                        top = Math.min(top, j - 1);
                        bottom = j - 1;
                    }
                    result = Math.min(result, lt[i][n] + (right - left + 1) * (bottom - top + 1) + lb[j][n]);
                }
            }
        }
        if (m >= 2 && n >= 2) for (int i = 1; i < m; i++) for (int j = 1; j < n; j++) result = Math.min(result, Math.min(lt[i][n] + lb[i][j] + rb[i][j], lt[i][j] + rt[i][j] + lb[i][n]));
        return result;
    }

    private int[][] minimumArea(int[][] a) {
        int m = a.length, n = a[0].length, dp[][] = new int[m + 1][n + 1], border[][] = new int[n][3];
        for (int j = 0; j < n; j++) border[j][0] = -1;
        for (int i = 0, left = -1, right = 0, r = 0, l = 0; i < m; i++, left = -1, right = 0) {
            for (int j = 0; j < n; j++) {
                if (a[i][j] == 1) {
                    left = left < 0 ? j : left;
                    right = j;
                }
                int[] pre = border[j];
                if (left < 0) dp[i + 1][j + 1] = dp[i][j + 1];
                else if (pre[0] < 0) { 
                    dp[i + 1][j + 1] = right - left + 1;
                    border[j][0] = i;
                    border[j][1] = left;
                    border[j][2] = right;
                } else {
                    dp[i + 1][j + 1] = ((r = Math.max(pre[2], right)) - (l = Math.min(pre[1], left)) + 1) * (i - pre[0] + 1);
                    border[j][1] = l;
                    border[j][2] = r;
                }
            }
        }
        return dp;
    }

    private int[][] rotate(int[][] a) {
        int m = a.length, n = a[0].length, result[][] = new int[n][m];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result[j][m - 1 - i] = a[i][j];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumSum(self, grid: List[List[int]]) -> int:
        rotate = lambda a: list(zip(*reversed(a)))
        def helper(a: List[List[int]]) -> int:
            m, n, result = len(a), len(a[0]), inf

            def min_area(a: List[List[int]]) -> List[List[int]]:
                m, n = len(a), len(a[0])
                dp, boarder = [[0] * (n + 1) for _ in range(m + 1)], [(-1, 0, 0)] * n
                for i, row in enumerate(a):
                    left, right = -1, 0
                    for j, val in enumerate(row):
                        if val:
                            left, right = j if left < 0 else left, j
                        pre_top, pre_left, pre_right = boarder[j]
                        if left < 0:
                            dp[i + 1][j + 1] = dp[i][j + 1]
                        elif pre_top < 0:
                            dp[i + 1][j + 1], boarder[j] = right - left + 1, (i, left, right)
                        else:
                            dp[i + 1][j + 1], boarder[j] = ((r := max(right, pre_right)) - (l := min(left, pre_left)) + 1) * (i - pre_top + 1), (pre_top, l, r)
                return dp
            lr, lt, lb, rb, rt = [None] * m, min_area(a), rotate(rotate(rotate(min_area(rotate(a))))), rotate(rotate(min_area(rotate(rotate(a))))), rotate(min_area(rotate(rotate(rotate(a)))))
            for i, row in enumerate(a):
                left, right = -1, 0
                for j, val in enumerate(row):
                    if val:
                        left, right = j if left < 0 else left, j
                lr[i] = (left, right)
            if m > 2:
                for i in range(1, m):
                    left, right, top, bottom = n, -1, m, -1
                    for j in range(i + 1, m):
                        if (l := lr[j - 1][0]) > -1:
                            left, right, top, bottom = min(left, l), max(right, r := lr[j - 1][1]), min(top, j - 1), j - 1
                        result = min(result, lt[i][n] + (right - left + 1) * (bottom - top + 1) + lb[j][n])
            if m > 1 and n > 1:
                for i in range(1, m):
                    for j in range(1, n):
                        result = min(result, lt[i][n] + lb[i][j] + rb[i][j], lt[i][j] + rt[i][j] + lb[i][n])
            return result
        return min(helper(grid), helper(rotate(grid)))
```
