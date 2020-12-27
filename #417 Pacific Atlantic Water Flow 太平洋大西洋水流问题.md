# 417 Pacific Atlantic Water Flow 太平洋大西洋水流问题

__Description__:
Given an m x n matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.

Water can only flow in four directions (up, down, left, or right) from a cell to another one with height equal or lower.

Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.

__Note:__

The order of returned grid coordinates does not matter.
Both m and n are less than 150.

__Example:__

Given the following 5x5 matrix:

```C
  Pacific ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * Atlantic
```

Return:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (positions with parentheses in above matrix).

__题目描述__:
给定一个 m x n 的非负整数矩阵来表示一片大陆上各个单元格的高度。“太平洋”处于大陆的左边界和上边界，而“大西洋”处于大陆的右边界和下边界。

规定水流只能按照上、下、左、右四个方向流动，且只能从高到低或者在同等高度上流动。

请找出那些水流既可以流动到“太平洋”，又能流动到“大西洋”的陆地单元的坐标。

__提示：__

输出坐标的顺序不重要
m 和 n 都小于150

__示例 :__

给定下面的 5x5 矩阵:

```C
  太平洋 ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * 大西洋
```

返回:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (上图中带括号的单元).

__思路__:

dfs
从边界开始搜索, 只有搜索到不比当前位置低的才继续搜索
时间复杂度O(mn), 空间复杂度O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& matrix) 
    {
        vector<vector<int>> result;
        if (matrix.empty() or matrix[0].empty()) return result;
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<bool>> p(m, vector<bool>(n)), a(m, vector<bool>(n));
        for (int i = 0; i < m; i++) 
        {
            dfs(matrix, i, 0, p, matrix[i][0]);
            dfs(matrix, i, n - 1, a, matrix[i][n - 1]);
        }
        for (int i = 0; i < n; i++) 
        {
            dfs(matrix, 0, i, p, matrix[0][i]);
            dfs(matrix, m - 1, i, a, matrix[m - 1][i]); 
        }
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if(p[i][j] && a[i][j]) result.push_back({i, j});
        return result;
    }
private:
    void dfs(vector<vector<int>>& matrix, int x, int y, vector<vector<bool>>& ocean, int pre) 
    {
        if (x < 0 or y < 0 or x >= matrix.size() or y >= matrix[0].size() or ocean[x][y] or matrix[x][y] < pre) return;
        ocean[x][y] = true;
        dfs(matrix, x + 1, y, ocean, matrix[x][y]);
        dfs(matrix, x - 1, y, ocean, matrix[x][y]);
        dfs(matrix, x, y + 1, ocean, matrix[x][y]);
        dfs(matrix, x, y - 1, ocean, matrix[x][y]);
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> pacificAtlantic(int[][] matrix) {
        List<List<Integer>> result = new ArrayList<>();
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return result;
        int m = matrix.length, n = matrix[0].length;
        boolean[][] p = new boolean[m][n], a = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            dfs(matrix, i, 0, p, matrix[i][0]);
            dfs(matrix, i, n - 1, a, matrix[i][n - 1]);
        }
        for (int i = 0; i < n; i++) {
            dfs(matrix, 0, i, p, matrix[0][i]);
            dfs(matrix, m - 1, i, a, matrix[m - 1][i]); 
        }
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if(p[i][j] && a[i][j]) result.add(Arrays.asList(i, j));
        return result;
    }
    
    private void dfs(int[][] matrix, int x, int y, boolean[][] ocean, int pre) {
        if (x < 0 || y < 0 || x >= matrix.length || y >= matrix[0].length || ocean[x][y] || matrix[x][y] < pre) return;
        ocean[x][y] = true;
        dfs(matrix, x + 1, y, ocean, matrix[x][y]);
        dfs(matrix, x - 1, y, ocean, matrix[x][y]);
        dfs(matrix, x, y + 1, ocean, matrix[x][y]);
        dfs(matrix, x, y - 1, ocean, matrix[x][y]);
    }
}
```

__Python__:

```Python
class Solution:
    def pacificAtlantic(self, matrix: List[List[int]]) -> List[List[int]]:
        result = []
        if not matrix or len(matrix) == 0:
            return result
        directions, m, n, p, a  = [[0,1], [0,-1], [1, 0], [-1,0]], len(matrix), len(matrix[0]), [[0] * len(matrix[0]) for _ in range(len(matrix))], [[0] * len(matrix[0]) for _ in range(len(matrix))]
        def dfs(row: int, col: int, ocean: List[List[int]]) -> None:
            if not ocean[row][col]:
                ocean[row][col] = 1
                for direction in directions:
                    x, y = row + direction[0], col + direction[1]
                    if x >= m or x < 0  or y >= n or y < 0 or matrix[x][y] < matrix[row][col]:
                        continue
                    dfs(x, y, ocean)
        for i in range(m):
            dfs(i, 0, p)
            dfs(i, n - 1, a)
        for j in range(n):
            dfs(0, j, p)
            dfs(m-1, j, a)
        for i in range(m):
            for j in range(n):
                if a[i][j] and p[i][j]:
                    result.append([i,j])
        return result
```
