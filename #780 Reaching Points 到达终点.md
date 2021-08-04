# 780 Reaching Points 到达终点

__Description__:
Given four integers sx, sy, tx, and ty, return true if it is possible to convert the point (sx, sy) to the point (tx, ty) through some operations, or false otherwise.

The allowed operation on some point (x, y) is to convert it to either (x, x + y) or (x + y, y).

__Example:__

Example 1:

Input: sx = 1, sy = 1, tx = 3, ty = 5
Output: true
Explanation:
One series of moves that transforms the starting point to the target is:

```text
(1, 1) -> (1, 2)
(1, 2) -> (3, 2)
(3, 2) -> (3, 5)
```

Example 2:

Input: sx = 1, sy = 1, tx = 2, ty = 2
Output: false

Example 3:

Input: sx = 1, sy = 1, tx = 1, ty = 1
Output: true

__Constraints:__

1 <= sx, sy, tx, ty <= 10^9

__题目描述__:
从点 (x, y) 可以转换到 (x, x + y)  或者 (x + y, y)。

给定一个起点 (sx, sy) 和一个终点 (tx, ty)，如果通过一系列的转换可以从起点到达终点，则返回 True ，否则返回 False。

__示例 :__

示例 1:
输入: sx = 1, sy = 1, tx = 3, ty = 5
输出: True
解释:
可以通过以下一系列转换从起点转换到终点：

```text
(1, 1) -> (1, 2)
(1, 2) -> (3, 2)
(3, 2) -> (3, 5)
```

示例 2:

输入: sx = 1, sy = 1, tx = 2, ty = 2
输出: False

示例 3:

输入: sx = 1, sy = 1, tx = 1, ty = 1
输出: True

__注意:__

sx, sy, tx, ty 是范围在 [1, 10^9] 的整数。

__思路__:

数学
类似欧几里得辗转相除法
每次选择 tx, ty 中较大的一个
比如 tx > ty, tx -= max((tx - sx) // ty, 1) * ty, 也可以用取模, tx %= ty
时间复杂度为 O(lgn), 空间复杂度为 O(1), 其中 n = max(tx, ty)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool reachingPoints(int sx, int sy, int tx, int ty) 
    {
        while (tx > 0 and ty > 0) 
        {
            if (sx == tx and sy == ty) return true;
            if (tx > ty) tx -= max((tx - sx) / ty, 1) * ty;
            else ty -= max((ty - sy) / tx, 1) * tx;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean reachingPoints(int sx, int sy, int tx, int ty) {
        while (tx > 0 && ty > 0) {
            if (tx == sx && ty == sy) return true;
            if (tx > ty) tx -= Math.max((tx - sx) / ty, 1) * ty;
            else ty -= Math.max((ty - sy) / tx, 1) * tx;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def reachingPoints(self, sx: int, sy: int, tx: int, ty: int) -> bool:
        while tx > 0 and ty > 0:
            if tx == sx and ty == sy:
                return True
            if tx > ty:
                tx -= max((tx - sx) // ty, 1) * ty
            else:
                ty -= max((ty - sy) // tx, 1) * tx
        return False
```
