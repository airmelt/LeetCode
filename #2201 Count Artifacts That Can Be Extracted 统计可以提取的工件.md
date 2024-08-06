# 2201 Count Artifacts That Can Be Extracted 统计可以提取的工件

__Description:__

There is an `n x n` __0-indexed__ grid with some artifacts buried in it. You are given the integer `n` and a __0-indexed__ 2D integer array `artifacts` describing the positions of the rectangular artifacts where `artifacts[i] = [r1i, c1i, r2i, c2i]` denotes that the `i ^ th` artifact is buried in the subgrid where:

- `(r1i, c1i)` is the coordinate of the __top-left__ cell of the `i ^ th` artifact and
- `(r2i, c2i)` is the coordinate of the __bottom-right__ cell of the `i ^ th` artifact.

You will excavate some cells of the grid and remove all the mud from them. If the cell has a part of an artifact buried underneath, it will be uncovered. If all the parts of an artifact are uncovered, you can extract it.

Given a __0-indexed__ 2D integer array `dig` where `dig[i] = [ri, ci]` indicates that you will excavate the cell `(ri, ci)`, return _the number of artifacts that you can extract_.

The test cases are generated such that:

- No two artifacts overlap.
- Each artifact only covers at most `4` cells.
- The entries of `dig` are unique.

__Example:__

Example 1:

![2201-1](https://assets.leetcode.com/uploads/2019/09/16/untitled-diagram.jpg)

```text
Input: n = 2, artifacts = [[0,0,0,0],[0,1,1,1]], dig = [[0,0],[0,1]]
Output: 1
Explanation: 
The different colors represent different artifacts. Excavated cells are labeled with a 'D' in the grid.
There is 1 artifact that can be extracted, namely the red artifact.
The blue artifact has one part in cell (1,1) which remains uncovered, so we cannot extract it.
Thus, we return 1.
```

Example 2:

![2201-2](https://assets.leetcode.com/uploads/2019/09/16/untitled-diagram-1.jpg)

```text
Input: n = 2, artifacts = [[0,0,0,0],[0,1,1,1]], dig = [[0,0],[0,1],[1,1]]
Output: 2
Explanation: Both the red and blue artifacts have all parts uncovered (labeled with a 'D') and can be extracted, so we return 2.
```

__Constraints:__

- `1 <= n <= 1000`
- `1 <= artifacts.length, dig.length <= min(n ^ 2, 10 ^ 5)`
- `artifacts[i].length == 4`
- `dig[i].length == 2`
- `0 <= r1i, c1i, r2i, c2i, ri, ci <= n - 1`
- `r1i <= r2i`
- `c1i <= c2i`
- No two artifacts will overlap.
- The number of cells covered by an artifact is __at most__ `4`.
- The entries of `dig` are unique.

__题目描述:__

存在一个 `n x n` 大小、下标从 __0__ 开始的网格，网格中埋着一些工件。给你一个整数 `n` 和一个下标从 __0__ 开始的二维整数数组 `artifacts` ， `artifacts` 描述了矩形工件的位置，其中 `artifacts[i] = [r1i, c1i, r2i, c2i]` 表示第 `i` 个工件在子网格中的填埋情况:

- `(r1i, c1i)` 是第 `i` 个工件 __左上__ 单元格的坐标，且
- `(r2i, c2i)` 是第 `i` 个工件 __右下__ 单元格的坐标。

你将会挖掘网格中的一些单元格，并清除其中的填埋物。如果单元格中埋着工件的一部分，那么该工件这一部分将会裸露出来。如果一个工件的所有部分都都裸露出来，你就可以提取该工件。

给你一个下标从 __0__ 开始的二维整数数组 `dig` ，其中 `dig[i] = [ri, ci]` 表示你将会挖掘单元格 `(ri, ci)` ，返回你可以提取的工件数目。

生成的测试用例满足：

- 不存在重叠的两个工件。
- 每个工件最多只覆盖 `4` 个单元格。
- `dig` 中的元素互不相同。

__示例:__

示例 1：

![2201-3](https://assets.leetcode.com/uploads/2019/09/16/untitled-diagram.jpg)

```text
输入：n = 2, artifacts = [[0,0,0,0],[0,1,1,1]], dig = [[0,0],[0,1]]
输出：1
解释： 
不同颜色表示不同的工件。挖掘的单元格用 'D' 在网格中进行标记。
有 1 个工件可以提取，即红色工件。
蓝色工件在单元格 (1,1) 的部分尚未裸露出来，所以无法提取该工件。
因此，返回 1 。
```

示例 2：

![2201-4](https://assets.leetcode.com/uploads/2019/09/16/untitled-diagram-1.jpg)

```text
输入：n = 2, artifacts = [[0,0,0,0],[0,1,1,1]], dig = [[0,0],[0,1],[1,1]]
输出：2
解释：红色工件和蓝色工件的所有部分都裸露出来（用 'D' 标记），都可以提取。因此，返回 2 。
```

__提示：__

- `1 <= n <= 1000`
- `1 <= artifacts.length, dig.length <= min(n ^ 2, 10 ^ 5)`
- `artifacts[i].length == 4`
- `dig[i].length == 2`
- `0 <= r1i, c1i, r2i, c2i, ri, ci <= n - 1`
- `r1i <= r2i`
- `c1i <= c2i`
- 不存在重叠的两个工件
- 每个工件 __最多__ 只覆盖 `4` 个单元格
- `dig` 中的元素互不相同

__思路:__

```text
模拟
先将所有挖过的单元格标记
再遍历所有工件, 判断是否所有单元格都被挖过
时间复杂度为 O(N + M), 空间复杂度为 O(M), M 为挖过的单元格个数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int digArtifacts(int n, vector<vector<int>>& artifacts, vector<vector<int>>& dig) 
    {
        int result = 0;
        vector<vector<bool>> visited(n, vector<bool>(n));
        for (const auto& d : dig) visited[d.front()][d.back()] = true;
        for (const auto& a : artifacts) 
        {
            bool flag = true;
            for (int i = a[0]; i <= a[2] and flag; i++) for (int j = a[1]; j <= a[3] and flag; j++) flag = visited[i][j];
            result += flag;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int digArtifacts(int n, int[][] artifacts, int[][] dig) {
        int result = 0;
        var visited = new boolean[n][n];
        for (var d : dig) visited[d[0]][d[1]] = true;
        for (var a : artifacts) {
            boolean flag = true;
            for (int i = a[0]; i <= a[2] && flag; i++) for (int j = a[1]; j <= a[3] && flag; j++) flag = visited[i][j];
            if (flag) ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def digArtifacts(self, n: int, artifacts: List[List[int]], dig: List[List[int]]) -> int:
        return 0 if not (dig := set(tuple(i) for i in dig)) else sum((a[0], a[1]) in dig if a[0] == a[2] and a[1] == a[3] else all((a[0], a[1] + i) in dig for i in range(a[3] - a[1] + 1)) if a[0] == a[2] else all((a[0] + i, a[1]) in dig for i in range(a[2] - a[0] + 1)) if a[1] == a[3] else (a[0], a[1]) in dig and (a[2], a[3]) in dig and (a[0] + 1, a[1]) in dig and (a[0], a[1] + 1) in dig for a in artifacts)
```
