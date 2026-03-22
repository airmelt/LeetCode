# 2713 Maximum Strictly Increasing Cells in a Matrix 矩阵中严格递增的单元格数

__Description:__

Given a __1-indexed__ `m x n` integer matrix `mat`, you can select any cell in the matrix as your __starting cell__.

From the starting cell, you can move to any other cell in the same row or column, but only if the value of the destination cell is strictly greater than the value of the current cell. You can repeat this process as many times as possible, moving from cell to cell until you can no longer make any moves.

Your task is to find the maximum number of cells that you can visit in the matrix by starting from some cell.

Return an integer denoting the maximum number of cells that can be visited.

__Example:__

Example 1:

![2713-1](https://assets.leetcode.com/uploads/2023/04/23/diag1drawio.png)

```text
Input: mat = [[3,1],[3,4]]
Output: 2
Explanation: The image shows how we can visit 2 cells starting from row 1, column 2. It can be shown that we cannot visit more than 2 cells no matter where we start from, so the answer is 2.
```

Example 2:

![2713-2](https://assets.leetcode.com/uploads/2023/04/23/diag3drawio.png)

```text
Input: mat = [[1,1],[1,1]]
Output: 1
Explanation: Since the cells must be strictly increasing, we can only visit one cell in this example.
```

Example 3:

![2713-3](https://assets.leetcode.com/uploads/2023/04/23/diag4drawio.png)

```text
Input: mat = [[3,1,6],[-9,5,7]]
Output: 4
Explanation: The image above shows how we can visit 4 cells starting from row 2, column 1. It can be shown that we cannot visit more than 4 cells no matter where we start from, so the answer is 4.
```

__Constraints:__

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `-10 ^ 5 <= mat[i][j] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __1__ 开始、大小为 `m x n` 的整数矩阵 `mat`，你可以选择任一单元格作为 __起始单元格__ 。

从起始单元格出发，你可以移动到 同一行或同一列 中的任何其他单元格，但前提是目标单元格的值 严格大于 当前单元格的值。

你可以多次重复这一过程，从一个单元格移动到另一个单元格，直到无法再进行任何移动。

请你找出从某个单元开始访问矩阵所能访问的 单元格的最大数量 。

返回一个表示可访问单元格最大数量的整数。

__示例:__

示例 1：

![2713-4](https://assets.leetcode.com/uploads/2023/04/23/diag1drawio.png)

```text
输入：mat = [[3,1],[3,4]]
输出：2
解释：上图展示了从第 1 行、第 2 列的单元格开始，可以访问 2 个单元格。可以证明，无论从哪个单元格开始，最多只能访问 2 个单元格，因此答案是 2 。
```

示例 2：

![2713-5](https://assets.leetcode.com/uploads/2023/04/23/diag3drawio.png)

输入：mat = [[1,1],[1,1]]
输出：1
解释：由于目标单元格必须严格大于当前单元格，在本示例中只能访问 1 个单元格。

示例 3：

![2713-6](https://assets.leetcode.com/uploads/2023/04/23/diag4drawio.png)

```text
输入：mat = [[3,1,6],[-9,5,7]]
输出：4
解释：上图展示了从第 2 行、第 1 列的单元格开始，可以访问 4 个单元格。可以证明，无论从哪个单元格开始，最多只能访问 4 个单元格，因此答案是 4 。
```

__提示：__

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `-10 ^ 5 <= mat[i][j] <= 10 ^ 5`

__思路:__

```text
动态规划
设 dp[i][j] 表示到达 mat[i][j] 时访问的最大数量
返回 max(dp) 即可
不需要知道从哪儿转移来, 记录每一行每一列的最大值
由于相同的值需要同时更新
按照从小到大的顺序记录所有值及其对应的位置
r 记录该行的 dp 的最大值
c 记录该列的 dp 的最大值
由于每一次都是计算比 mat[i][j] 小的值
则 dp[i][j] = max(r[i], c[j]) + 1
最后返回 max(r) 或者 max(c) 是相等的
时间复杂度为 O(MNlogMN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxIncreasingCells(vector<vector<int>>& mat) 
    {
        int m = mat.size(), n = mat.front().size();
        map<int, vector<pair<int, int>>> graph;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) graph[mat[i][j]].emplace_back(i, j);
        vector<int> r(m), c(n);
        for (const auto& [_, pos] : graph) 
        {
            vector<int> dp;
            for (const auto& [i, j] : pos) dp.emplace_back(max(r[i], c[j]) + 1);
            for (int k = 0; k < pos.size(); k++) 
            {
                r[pos[k].first] = max(r[pos[k].first], dp[k]);
                c[pos[k].second] = max(c[pos[k].second], dp[k]);
            }
        }
        return *max_element(r.begin(), r.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int maxIncreasingCells(int[][] mat) {
        int m = mat.length, n = mat[0].length, r[] = new int[m], c[] = new int[n];
        var graph = new TreeMap<Integer, List<int[]>>();
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) graph.computeIfAbsent(mat[i][j], k -> new ArrayList<>()).add(new int[]{ i, j });
        for (var pos : graph.values()) {
            int dp[] = new int[pos.size()], p = pos.size();
            for (int k = 0; k < p; k++) dp[k] = Math.max(r[pos.get(k)[0]], c[pos.get(k)[1]]) + 1;
            for (int k = 0; k < p; k++) {
                r[pos.get(k)[0]] = Math.max(r[pos.get(k)[0]], dp[k]);
                c[pos.get(k)[1]] = Math.max(c[pos.get(k)[1]], dp[k]);
            }
        }
        return Arrays.stream(r).max().getAsInt();
    }
}
```

__Python__:

```Python
class Solution:
    def maxIncreasingCells(self, mat: List[List[int]]) -> int:
        graph, r, c = defaultdict(list), [0] * len(mat), [0] * len(mat[0])
        for i, row in enumerate(mat):
            for j, v in enumerate(row):
                graph[v].append((i, j))
        for _, pos in sorted(graph.items(), key=lambda p: p[0]):
            dp = [max(r[i], c[j]) + 1 for i, j in pos]
            for (i, j), v in zip(pos, dp):
                r[i], c[j] = max(r[i], v), max(c[j], v)
        return max(r)
```
