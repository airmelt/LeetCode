# 885 Spiral Matrix III 螺旋矩阵 III

__Description__:
You start at the cell (rStart, cStart) of an rows x cols grid facing east. The northwest corner is at the first row and column in the grid, and the southeast corner is at the last row and column.

You will walk in a clockwise spiral shape to visit every position in this grid. Whenever you move outside the grid's boundary, we continue our walk outside the grid (but may return to the grid boundary later.). Eventually, we reach all rows * cols spaces of the grid.

Return an array of coordinates representing the positions of the grid in the order you visited them.

__Example:__

Example 1:

![Spiral Matrix 1](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/24/example_1.png)

Input: rows = 1, cols = 4, rStart = 0, cStart = 0
Output: [[0,0],[0,1],[0,2],[0,3]]

Example 2:

![Spiral Matrix 2](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/24/example_2.png)

Input: rows = 5, cols = 6, rStart = 1, cStart = 4
Output: [[1,4],[1,5],[2,5],[2,4],[2,3],[1,3],[0,3],[0,4],[0,5],[3,5],[3,4],[3,3],[3,2],[2,2],[1,2],[0,2],[4,5],[4,4],[4,3],[4,2],[4,1],[3,1],[2,1],[1,1],[0,1],[4,0],[3,0],[2,0],[1,0],[0,0]]

__Constraints:__

1 <= rows, cols <= 100
0 <= rStart < rows
0 <= cStart < cols

__题目描述__:
在 R 行 C 列的矩阵上，我们从 (r0, c0) 面朝东面开始

这里，网格的西北角位于第一行第一列，网格的东南角位于最后一行最后一列。

现在，我们以顺时针按螺旋状行走，访问此网格中的每个位置。

每当我们移动到网格的边界之外时，我们会继续在网格之外行走（但稍后可能会返回到网格边界）。

最终，我们到过网格的所有 R * C 个空间。

按照访问顺序返回表示网格位置的坐标列表。

__示例 :__

示例 1：

![螺旋矩阵 1](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/08/24/example_1.png)

输入：R = 1, C = 4, r0 = 0, c0 = 0
输出：[[0,0],[0,1],[0,2],[0,3]]

示例 2：

![螺旋矩阵 2](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/08/24/example_2.png)

输入：R = 5, C = 6, r0 = 1, c0 = 4
输出：[[1,4],[1,5],[2,5],[2,4],[2,3],[1,3],[0,3],[0,4],[0,5],[3,5],[3,4],[3,3],[3,2],[2,2],[1,2],[0,2],[4,5],[4,4],[4,3],[4,2],[4,1],[3,1],[2,1],[1,1],[0,1],[4,0],[3,0],[2,0],[1,0],[0,0]]

__提示:__

1 <= R <= 100
1 <= C <= 100
0 <= r0 < R
0 <= c0 < C

__思路__:

模拟
按照题意, 每次选择一个方向前进, 超出边界的丢弃
时间复杂度为 O(max(r, c) ^ 2), 空间复杂度为 O(rc)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> spiralMatrixIII(int rows, int cols, int rStart, int cStart) 
    {
        const int dir[4][2] = { { 0, 1 }, { 1, 0 }, { 0, -1 }, { -1, 0 } };
        vector<vector<int>> result{ { rStart, cStart } };
        for (int i = 0, n = rows * cols; result.size() < n; i++) 
        {
            for (int j = 0; j < (i >> 1) + 1; j++) 
            {
                rStart += dir[i % 4][0];
                cStart += dir[i % 4][1];
                if (-1 < rStart and rStart < rows and -1 < cStart and cStart < cols) result.push_back({ rStart, cStart });
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] spiralMatrixIII(int rows, int cols, int rStart, int cStart) {
        int dir[][] = new int[][]{ { 0, 1 }, { 1, 0 }, { 0, -1 }, { -1, 0 } }, result[][] = new int[rows * cols][2], index = 1;
        result[0][0] = rStart;
        result[0][1] = cStart;
        for (int i = 0, n = rows * cols; index < n; i++) {
            for (int j = 0; j < (i >> 1) + 1; j++) {
                rStart += dir[i % 4][0];
                cStart += dir[i % 4][1];
                if (-1 < rStart && rStart < rows && -1 < cStart && cStart < cols) {
                    result[index][0] = rStart;
                    result[index++][1] = cStart;
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def spiralMatrixIII(self, rows: int, cols: int, rStart: int, cStart: int) -> List[List[int]]:
        d, result, i, n = [(0, 1), (1, 0), (0, -1), (-1, 0)], [[rStart, cStart]], 0, rows * cols
        while len(result) < n:
            for _ in range((i >> 1) + 1):
                rStart, cStart = rStart + d[i % 4][0], cStart + d[i % 4][1]
                if -1 < rStart < rows and -1 < cStart < cols:
                    result.append([rStart, cStart])
            i += 1
        return result
```
