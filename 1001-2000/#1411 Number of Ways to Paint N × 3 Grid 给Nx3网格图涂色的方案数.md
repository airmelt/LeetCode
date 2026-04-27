# 1411 Number of Ways to Paint N × 3 Grid 给Nx3网格图涂色的方案数

__Description:__

You have a `grid` of size `n x 3` and you want to paint each cell of the grid with exactly one of the three colors: __Red__, __Yellow,__ or __Green__ while making sure that no two adjacent cells have the same color (i.e., no two cells that share vertical or horizontal sides have the same color).

Given `n` the number of rows of the grid, return _the number of ways_ you can paint this `grid`. As the answer may grow large, the answer __must be__ computed modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

![1411-1](https://assets.leetcode.com/uploads/2020/03/26/e1.png)

```text
Input: n = 1
Output: 12
Explanation: There are 12 possible way to paint the grid as shown.
```

Example 2:

```text
Input: n = 5000
Output: 30228214
```

__Constraints:__

- `n == grid.length`
- `1 <= n <= 5000`

__题目描述:__

你有一个 `n x 3` 的网格图 `grid` ，你需要用 __红，黄，绿__ 三种颜色之一给每一个格子上色，且确保相邻格子颜色不同（也就是有相同水平边或者垂直边的格子颜色不同）。

给你网格图的行数 `n` 。

请你返回给 `grid` 涂色的方案数。由于答案可能会非常大，请你返回答案对 `10 ^ 9 + 7` 取余的结果。

__示例:__

示例 1：

```text
输入：n = 1
输出：12
解释：总共有 12 种可行的方法：
```

![1411-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/04/12/e1.png)

示例 2：

```text
输入：n = 2
输出：54
```

示例 3：

```text
输入：n = 3
输出：246
```

示例 4：

```text
输入：n = 7
输出：106494
```

示例 5：

```text
输入：n = 5000
输出：30228214
```

__提示：__

- `n == grid.length`
- `grid[i].length == 3`
- `1 <= n <= 5000`

__思路:__

```text
递推
不妨设, 红色为 0, 绿色为 1, 黄色为 2
从 n = 1 中可以将块分成两类
一种是 ABA: 010, 020, 101, 121, 202, 212, 共 6 种
一种是 ABC: 012, 021, 102, 120, 201, 210, 共 6 种
接下来考虑第二层如果是 ABA 形式, 接下来能放的 ABA 的有 3 种
比如 010, 第二层也为 ABA 的为 101, 121, 202, 其余类似
如果是 ABC 形式则只能为 102, 201 两种其余类似
那么接下来 ABA = 3 * ABA + 2 * ABC
类似分析可得 ABC = 2 * (ABA + ABC)
注意到上述递推式可以写成矩阵的形式 [a, b] = [3 * a + 2 * b, 2 * a + 2 * b] = [[3, 2], [2, 2]][a, b]
可以用矩阵快速幂将时间复杂度降低为 O(logN)
时间复杂度为 O(lgN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numOfWays(int n) 
    {
        long a = 6, b = 6, mod = 1e9 + 7;
        for (int i = 1; i < n; i++)
        {
            long cur = a;
            a = (a * 3 + (b << 1)) % mod;
            b = ((cur + b) << 1) % mod;
        }
        return (a + b) % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int numOfWays(int n) {
        long a = 6, b = 6, mod = 1_000_000_007;
        for (int i = 1; i < n; i++) {
            long cur = a;
            a = (a * 3 + (b << 1)) % mod;
            b = ((cur + b) << 1) % mod;
        }
        return (int)((a + b) % mod);
    }
}
```

__Python__:

```Python
class Solution:
    def numOfWays(self, n: int) -> int:
        multi, mod = lambda x, y: [[((x[0][0] * y[0][0]) + (x[0][1] * y[1][0])), ((x[0][0] * y[0][1]) + (x[0][1] * y[1][1]))], [((x[1][0] * y[0][0]) + (x[1][1] * y[1][0])), ((x[1][0] * y[0][1]) + (x[1][1] * y[1][1]))]], 10 ** 9 + 7

        def power(x: List[List[int]], n: int) -> List[List[int]]:
            result = [[1, 0], [0, 1]]
            while n:
                if n & 1:
                    result = multi(result, x)
                n >>= 1
                x = multi(x, x)
            return result
        return 3 * ((x := power([[3, 2], [2, 2]], n))[0][1] + x[1][1]) % mod
```
