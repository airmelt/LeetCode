# 1992 Find All Groups of Farmland 找到所有的农场组

__Description:__

You are given a __0-indexed__ `m x n` binary matrix `land` where a `0` represents a hectare of forested land and a `1` represents a hectare of farmland.

To keep the land organized, there are designated rectangular areas of hectares that consist entirely of farmland. These rectangular areas are called groups. No two groups are adjacent, meaning farmland in one group is not four-directionally adjacent to another farmland in a different group.

`land` can be represented by a coordinate system where the top left corner of `land` is `(0, 0)` and the bottom right corner of `land` is `(m-1, n-1)`. Find the coordinates of the top left and bottom right corner of each __group__ of farmland. A __group__ of farmland with a top left corner at `(r1, c1)` and a bottom right corner at `(r2, c2)` is represented by the 4-length array `[r1, c1, r2, c2].`

Return _a 2D array containing the 4-length arrays described above for each __group__ of farmland in_ `land`_. If there are no groups of farmland, return an empty array. You may return the answer in __any order___.

__Example:__

Example 1:

![1992-1](https://assets.leetcode.com/uploads/2021/07/27/screenshot-2021-07-27-at-12-23-15-copy-of-diagram-drawio-diagrams-net.png)

```text
Input: land = [[1,0,0],[0,1,1],[0,1,1]]
Output: [[0,0,0,0],[1,1,2,2]]
Explanation:
The first group has a top left corner at land[0][0] and a bottom right corner at land[0][0].
The second group has a top left corner at land[1][1] and a bottom right corner at land[2][2].
```

Example 2:

![1992-2](https://assets.leetcode.com/uploads/2021/07/27/screenshot-2021-07-27-at-12-30-26-copy-of-diagram-drawio-diagrams-net.png)

```text
Input: land = [[1,1],[1,1]]
Output: [[0,0,1,1]]
Explanation:
The first group has a top left corner at land[0][0] and a bottom right corner at land[1][1].
```

Example 3:

![1992-3](https://assets.leetcode.com/uploads/2021/07/27/screenshot-2021-07-27-at-12-32-24-copy-of-diagram-drawio-diagrams-net.png)

```text
Input: land = [[0]]
Output: []
Explanation:
There are no groups of farmland.
```

__Constraints:__

- `m == land.length`
- `n == land[i].length`
- `1 <= m, n <= 300`
- `land` consists of only `0`'s and `1`'s.
- Groups of farmland are __rectangular__ in shape.

__题目描述:__

给你一个下标从 __0__ 开始，大小为 `m x n` 的二进制矩阵 `land` ，其中 `0` 表示一单位的森林土地， `1` 表示一单位的农场土地。

为了让农场保持有序，农场土地之间以矩形的 农场组 的形式存在。每一个农场组都 仅 包含农场土地。且题目保证不会有两个农场组相邻，也就是说一个农场组中的任何一块土地都 不会 与另一个农场组的任何一块土地在四个方向上相邻。

`land` 可以用坐标系统表示，其中 `land` 左上角坐标为 `(0, 0)` ，右下角坐标为 `(m-1, n-1)` 。请你找到所有 _农场组_ 最左上角和最右下角的坐标。一个左上角坐标为 `(r1, c1)` 且右下角坐标为 `(r2, c2)` 的 __农场组__ 用长度为 4 的数组 `[r1, c1, r2, c2]` 表示。

请你返回一个二维数组，它包含若干个长度为 4 的子数组，每个子数组表示 `land` 中的一个 __农场组__ 。如果没有任何农场组，请你返回一个空数组。可以以 __任意顺序__ 返回所有农场组。

__示例:__

示例 1：

![1992-4](https://assets.leetcode.com/uploads/2021/07/27/screenshot-2021-07-27-at-12-23-15-copy-of-diagram-drawio-diagrams-net.png)

```text
输入：land = [[1,0,0],[0,1,1],[0,1,1]]
输出：[[0,0,0,0],[1,1,2,2]]
解释：
第一个农场组的左上角为 land[0][0] ，右下角为 land[0][0] 。
第二个农场组的左上角为 land[1][1] ，右下角为 land[2][2] 。
```

示例 2：

![1992-5](https://assets.leetcode.com/uploads/2021/07/27/screenshot-2021-07-27-at-12-30-26-copy-of-diagram-drawio-diagrams-net.png)

```text
输入：land = [[1,1],[1,1]]
输出：[[0,0,1,1]]
解释：
第一个农场组左上角为 land[0][0] ，右下角为 land[1][1] 。
```

示例 3：

![1992-6](https://assets.leetcode.com/uploads/2021/07/27/screenshot-2021-07-27-at-12-32-24-copy-of-diagram-drawio-diagrams-net.png)

```text
输入：land = [[0]]
输出：[]
解释：
没有任何农场组。
```

__提示：__

- `m == land.length`
- `n == land[i].length`
- `1 <= m, n <= 300`
- `land` 只包含 `0` 和 `1` 。
- 农场组都是 __矩形__ 的形状。

__思路:__

```text
模拟
找到左上角的点
然后搜索为 1 的点直到找到右下角的点
将搜索到的点置为 0
并将左上角和右下角的点加入结果中
时间复杂度为 O(M ^ 2N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> findFarmland(vector<vector<int>>& land) 
    {
        vector<vector<int>> result;
        for (int i = 0, m = land.size(), n = land.front().size(); i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (land[i][j]) 
                {
                    int x = i, y = j;
                    while (x < m - 1 and land[x + 1][j]) ++x;
                    while (y < n - 1 and land[i][y + 1]) ++y;
                    result.emplace_back(vector<int>{i, j, x, y});
                    for (int row = i; row <= x; row++) for (int col = j; col <= y; col++) land[row][col] = 0;
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] findFarmland(int[][] land) {
        List<int[]> result = new ArrayList();
        for (int i = 0, m = land.length, n = land[0].length; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (land[i][j] != 0) {
                    int x = i, y = j;
                    while (x < m - 1 && land[x + 1][j] == 1) ++x;
                    while (y < n - 1 && land[i][y + 1] == 1) ++y;
                    result.add(new int[]{i, j, x, y});
                    for (int row = i; row <= x; row++) for (int col = j; col <= y; col++) land[row][col] = 0;
                }
            }
        }
        return result.toArray(new int[0][0]);
    }
}
```

__Python__:

```Python
class Solution:
    def findFarmland(self, land: List[List[int]]) -> List[List[int]]:
        result, m, n = [], len(land), len(land[0])
        for i in range(m):
            for j in range(n):
                if land[i][j]:
                    x, y = i, j
                    while x < m - 1 and land[x + 1][j]:
                        x += 1
                    while y < n - 1 and land[i][y + 1]:
                        y += 1
                    result.append([i, j, x, y])
                    for row in range(i, x + 1):
                        for col in range(j, y + 1):
                            land[row][col] = 0
        return result
```
