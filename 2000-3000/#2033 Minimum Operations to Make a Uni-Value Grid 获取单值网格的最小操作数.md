# 2033 Minimum Operations to Make a Uni-Value Grid 获取单值网格的最小操作数

__Description:__

You are given a 2D integer `grid` of size `m x n` and an integer `x`. In one operation, you can __add__ `x` to or __subtract__ `x` from any element in the `grid`.

A uni-value grid is a grid where all the elements of it are equal.

Return _the __minimum__ number of operations to make the grid __uni-value___. If it is not possible, return `-1`.

__Example:__

Example 1:

![2033-1](https://assets.leetcode.com/uploads/2021/09/21/gridtxt.png)

```text
Input: grid = [[2,4],[6,8]], x = 2
Output: 4
Explanation: We can make every element equal to 4 by doing the following: 
```

- Add x to 2 once.
- Subtract x from 6 once.
- Subtract x from 8 twice.
A total of 4 operations were used.

Example 2:

![2033-2](https://assets.leetcode.com/uploads/2021/09/21/gridtxt-1.png)

```text
Input: grid = [[1,5],[2,3]], x = 1
Output: 5
Explanation: We can make every element equal to 3.
```

Example 3:

![2033-3](https://assets.leetcode.com/uploads/2021/09/21/gridtxt-2.png)

```text
Input: grid = [[1,2],[3,4]], x = 2
Output: -1
Explanation: It is impossible to make every element equal.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `1 <= x, grid[i][j] <= 10 ^ 4`

__题目描述:__

给你一个大小为 `m x n` 的二维整数网格 `grid` 和一个整数 `x` 。每一次操作，你可以对 `grid` 中的任一元素 __加__ `x` 或 __减__ `x` 。

单值网格 是全部元素都相等的网格。

返回使网格化为单值网格所需的 __最小__ 操作数。如果不能，返回 `-1` 。

__示例:__

示例 1：

![2033-4](https://assets.leetcode.com/uploads/2021/09/21/gridtxt.png)

```text
输入：grid = [[2,4],[6,8]], x = 2
输出：4
解释：可以执行下述操作使所有元素都等于 4 ：
```

- 2 加 x 一次。
- 6 减 x 一次。
- 8 减 x 两次。
共计 4 次操作。

示例 2：

![2033-5](https://assets.leetcode.com/uploads/2021/09/21/gridtxt-1.png)

```text
输入：grid = [[1,5],[2,3]], x = 1
输出：5
解释：可以使所有元素都等于 3 。
```

示例 3：

![2033-6](https://assets.leetcode.com/uploads/2021/09/21/gridtxt-2.png)

```text
输入：grid = [[1,2],[3,4]], x = 2
输出：-1
解释：无法使所有元素相等。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `1 <= x, grid[i][j] <= 10 ^ 4`

__思路:__

```text
贪心
先检查每一个数和第一个数的差值是否能被 x 整除
排序之后, 中位数即为目标值
统计每个数到目标值的距离, 除以 x 即为操作数
时间复杂度为 O(MN), 空间复杂度为 O(MN), 可以使用快速选择算法来求中位数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<vector<int>>& grid, int x) 
    {
        int m = grid.size(), n = grid.front().size(), total = m * n, result = 0;
        vector<int> a(total);
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if ((grid[i][j] - grid.front().front()) % x) return -1;
                a[i * n + j] = grid[i][j];
            }
        }
        sort(a.begin(), a.end());
        for (int i = 0; i < total; i++) result += abs(a[i] - a[total >> 1]) / x;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[][] grid, int x) {
        int m = grid.length, n = grid[0].length, total = m * n, a[] = new int[total], result = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if ((grid[i][j] - grid[0][0]) % x != 0) return -1;
                a[i * n + j] = grid[i][j];
            }
        }
        Arrays.sort(a);
        for (int i = 0; i < total; i++) result += Math.abs(a[i] - a[total >>> 1]) / x;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, grid: List[List[int]], x: int) -> int:
        return -1 if (a := sorted(chain.from_iterable(grid))) and any((v - a[0]) % x for v in a) else sum(abs(v - a[len(a) >> 1]) // x for v in a)
```
