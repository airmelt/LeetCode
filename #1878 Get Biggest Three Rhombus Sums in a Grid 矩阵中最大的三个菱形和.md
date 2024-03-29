# 1878 Get Biggest Three Rhombus Sums in a Grid 矩阵中最大的三个菱形和

__Description:__

You are given an `m x n` integer matrix `grid`​​​.

A __rhombus sum__ is the sum of the elements that form __the__ __border__ of a regular rhombus shape in `grid`​​​. The rhombus must have the shape of a square rotated 45 degrees with each of the corners centered in a grid cell. Below is an image of four valid rhombus shapes with the corresponding colored cells that should be included in each __rhombus sum__:

![1878-1](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-desc-2.png)

Note that the rhombus can have an area of 0, which is depicted by the purple rhombus in the bottom right corner.

Return _the biggest three __distinct rhombus sums__ in the_ `grid` _in __descending order____. If there are less than three distinct values, return all of them_.

__Example:__

Example 1:

![1878-2](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-ex1.png)

```text
Input: grid = [[3,4,5,1,3],[3,3,4,2,3],[20,30,200,40,10],[1,5,5,4,1],[4,3,2,2,5]]
Output: [228,216,211]
Explanation: The rhombus shapes for the three biggest distinct rhombus sums are depicted above.
- Blue: 20 + 3 + 200 + 5 = 228
- Red: 200 + 2 + 10 + 4 = 216
- Green: 5 + 200 + 4 + 2 = 211
```

Example 2:

![1878-3](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-ex2.png)

```text
Input: grid = [[1,2,3],[4,5,6],[7,8,9]]
Output: [20,9,8]
Explanation: The rhombus shapes for the three biggest distinct rhombus sums are depicted above.
- Blue: 4 + 2 + 6 + 8 = 20
- Red: 9 (area 0 rhombus in the bottom right corner)
- Green: 8 (area 0 rhombus in the bottom middle)
```

Example 3:

```text
Input: grid = [[7,7,7]]
Output: [7]
Explanation: All three possible rhombus sums are the same, so return [7].
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `1 <= grid[i][j] <= 10 ^ 5`

__题目描述:__

给你一个 `m x n` 的整数矩阵 `grid` 。

__菱形和__ 指的是 `grid` 中一个正菱形 __边界__ 上的元素之和。本题中的菱形必须为正方形旋转45度，且四个角都在一个格子当中。下图是四个可行的菱形，每个菱形和应该包含的格子都用了相应颜色标注在图中。

![1878-4](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-desc-2.png)

注意，菱形可以是一个面积为 0 的区域，如上图中右下角的紫色菱形所示。

请你按照 __降序__ 返回 `grid` 中三个最大的 __互不相同的菱形和__ 。如果不同的和少于三个，则将它们全部返回。

__示例:__

示例 1：

![1878-5](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-ex1.png)

```text
输入：grid = [[3,4,5,1,3],[3,3,4,2,3],[20,30,200,40,10],[1,5,5,4,1],[4,3,2,2,5]]
输出：[228,216,211]
解释：最大的三个菱形和如上图所示。
- 蓝色：20 + 3 + 200 + 5 = 228
- 红色：200 + 2 + 10 + 4 = 216
- 绿色：5 + 200 + 4 + 2 = 211
```

示例 2：

![1878-6](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-ex2.png)

```text
输入：grid = [[1,2,3],[4,5,6],[7,8,9]]
输出：[20,9,8]
解释：最大的三个菱形和如上图所示。
- 蓝色：4 + 2 + 6 + 8 = 20
- 红色：9 （右下角红色的面积为 0 的菱形）
- 绿色：8 （下方中央面积为 0 的菱形）
```

示例 3：

```text
输入：grid = [[7,7,7]]
输出：[7]
解释：所有三个可能的菱形和都相同，所以返回 [7] 。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100`
- `1 <= grid[i][j] <= 10 ^ 5`

__思路:__

```text
前缀和
从数据范围来看, 使用 O(N ^ 3) 的算法也能通过
即使用暴力法, 枚举每一个菱形, 从每一个点开始向外进行扩展, 直到碰到矩阵的边界
可以用前缀和优化计算过程
pre1[i][j] 表示从右下到左上的所有元素的和
pre1[i + 1][j + 1] = pre1[i][j] + grid[i][j]
pre2[i][j] 表示从左下到右上的所有元素的和
pre2[i + 1][j + 1] = pre2[i][j + 2] + grid[i][j]
为了方便起见, 可以用有序集合来存储结果, 这样方便去重和取最大值
时间复杂度为 O(MN ^ 2), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> getBiggestThree(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size();
        vector<vector<int>> pre1(m + 2, vector<int>(n + 2)), pre2(m + 2, vector<int>(n + 2));
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                pre1[i + 1][j + 1] = pre1[i][j] + grid[i][j];
                pre2[i + 1][j + 1] = pre2[i][j + 2] + grid[i][j];
            }
        }
        set<int, greater<int>> s;
        for (int i = 1; i <= m; i++) 
        {
            for (int j = 1; j <= n; j++) 
            {
                s.insert(grid[i - 1][j - 1]);
                for (int k = 1; i - k >= 1 && i + k <= m && j - k >= 1 && j + k <= n; k++) s.insert(pre2[i][j - k] - pre2[i - k][j] + pre1[i][j + k] - pre1[i - k][j] + pre2[i + k][j] - pre2[i][j + k] + pre1[i + k][j] - pre1[i][j - k] - grid[i + k - 1][j - 1] + grid[i - k - 1][j - 1]);
            }
        }
        vector<int> result;
        if (s.size() == 1) result.emplace_back(*s.begin());
        else if (s.size() == 2)
        {
            result.emplace_back(*s.begin());
            s.erase(s.begin());
            result.emplace_back(*s.begin());
        }
        else
        {
            result.emplace_back(*s.begin());
            s.erase(s.begin());
            result.emplace_back(*s.begin());
            s.erase(s.begin());
            result.emplace_back(*s.begin());
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] getBiggestThree(int[][] grid) {
        int m = grid.length, n = grid[0].length, pre1[][] = new int[m + 2][n + 2], pre2[][] = new int[m + 2][n + 2];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                pre1[i + 1][j + 1] = pre1[i][j] + grid[i][j];
                pre2[i + 1][j + 1] = pre2[i][j + 2] + grid[i][j];
            }
        }
        TreeSet<Integer> set = new TreeSet<>();
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                set.add(grid[i - 1][j - 1]);
                for (int k = 1; i - k >= 1 && i + k <= m && j - k >= 1 && j + k <= n; k++) set.add(pre2[i][j - k] - pre2[i - k][j] + pre1[i][j + k] - pre1[i - k][j] + pre2[i + k][j] - pre2[i][j + k] + pre1[i + k][j] - pre1[i][j - k] - grid[i + k - 1][j - 1] + grid[i - k - 1][j - 1]);
            }
        }
        return set.size() == 1 ? new int[]{ set.pollLast() } : (set.size() == 2 ? new int[]{ set.pollLast(), set.pollLast() } : new int[]{ set.pollLast(), set.pollLast(), set.pollLast() });
    }
}
```

__Python__:

```Python
class Solution:
    def getBiggestThree(self, grid: List[List[int]]) -> List[int]:
        m, n, s = len(grid), len(grid[0]), set()
        pre1, pre2 = [[0] * (n + 2) for _ in range(m + 2)], [[0] * (n + 2) for _ in range(m + 2)]
        for i in range(m):
            for j in range(n):
                pre1[i + 1][j + 1], pre2[i + 1][j + 1] = pre1[i][j] + grid[i][j], pre2[i][j + 2] + grid[i][j]
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                s.add(grid[i - 1][j - 1])
                for k in range(1, min(m - i + 1, n - j + 1)):
                    if i - k < 1 or j - k < 1:
                        break
                    s.add(pre2[i][j - k] - pre2[i - k][j] + pre1[i][j + k] - pre1[i - k][j] + pre2[i + k][j] - pre2[i][j + k] + pre1[i + k][j] - pre1[i][j - k] - grid[i + k - 1][j - 1] + grid[i - k - 1][j - 1])
        return sorted(list(s), reverse=True)[:3]
```
