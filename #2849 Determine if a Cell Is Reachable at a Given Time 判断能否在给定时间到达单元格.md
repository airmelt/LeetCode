# 2849 Determine if a Cell Is Reachable at a Given Time 判断能否在给定时间到达单元格

__Description:__

You are given four integers `sx`, `sy`, `fx`, `fy`, and a __non-negative__ integer `t`.

In an infinite 2D grid, you start at the cell `(sx, sy)`. Each second, you __must__ move to any of its adjacent cells.

Return `true` _if you can reach cell_ `(fx, fy)` _after __exactly___ `t` ___seconds___, _or_ `false` _otherwise_.

A cell's adjacent cells are the 8 cells around it that share at least one corner with it. You can visit the same cell several times.

__Example:__

Example 1:

![2849-1](https://assets.leetcode.com/uploads/2023/08/05/example2.svg)

```text
Input: sx = 2, sy = 4, fx = 7, fy = 7, t = 6
Output: true
Explanation: Starting at cell (2, 4), we can reach cell (7, 7) in exactly 6 seconds by going through the cells depicted in the picture above.
```

Example 2:

![2849-2](https://assets.leetcode.com/uploads/2023/08/05/example1.svg)

```text
Input: sx = 3, sy = 1, fx = 7, fy = 3, t = 3
Output: false
Explanation: Starting at cell (3, 1), it takes at least 4 seconds to reach cell (7, 3) by going through the cells depicted in the picture above. Hence, we cannot reach cell (7, 3) at the third second.
```

__Constraints:__

- `1 <= sx, sy, fx, fy <= 10 ^ 9`
- `0 <= t <= 10 ^ 9`

__题目描述:__

给你四个整数 `sx`、 `sy`、 `fx`、 `fy` 以及一个 __非负整数__ `t` 。

在一个无限的二维网格中，你从单元格 `(sx, sy)` 开始出发。每一秒，你 __必须__ 移动到任一与之前所处单元格相邻的单元格中。

如果你能在 __恰好__ `t` __秒__ 后到达单元格 `(fx, fy)` ，返回 `true` ；否则，返回  `false` 。

单元格的 相邻单元格 是指该单元格周围与其至少共享一个角的 8 个单元格。你可以多次访问同一个单元格。

__示例:__

示例 1：

![2849-3](https://assets.leetcode.com/uploads/2023/08/05/example2.svg)

```text
输入：sx = 2, sy = 4, fx = 7, fy = 7, t = 6
输出：true
解释：从单元格 (2, 4) 开始出发，穿过上图标注的单元格，可以在恰好 6 秒后到达单元格 (7, 7) 。
```

示例 2：

![2849-4](https://assets.leetcode.com/uploads/2023/08/05/example1.svg)

```text
输入：sx = 3, sy = 1, fx = 7, fy = 3, t = 3
输出：false
解释：从单元格 (3, 1) 开始出发，穿过上图标注的单元格，至少需要 4 秒后到达单元格 (7, 3) 。 因此，无法在 3 秒后到达单元格 (7, 3) 。
```

__提示：__

- `1 <= sx, sy, fx, fy <= 10 ^ 9`
- `0 <= t <= 10 ^ 9`

__思路:__

```text
贪心
要保证能够到达
t 必须不小于两个点对应差值的较大值
还有一种额外情况
t == 1 时
不能直接回到原点
其他情况只要在终点附近绕圈就能消耗完 t
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isReachableAtTime(int sx, int sy, int fx, int fy, int t) 
    {
        return t >= max(abs(fx - sx), abs(fy - sy)) and not (t == 1 and sx == fx and sy == fy);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isReachableAtTime(int sx, int sy, int fx, int fy, int t) {
        return t >= Math.max(Math.abs(fx - sx), Math.abs(fy - sy)) && !(t == 1 && sx == fx && sy == fy);
    }
}
```

__Python__:

```Python
class Solution:
    def isReachableAtTime(self, sx: int, sy: int, fx: int, fy: int, t: int) -> bool:
        return t >= max(abs(fx - sx), abs(fy - sy)) and not (t == 1 and (sx, sy) == (fx, fy))
```
