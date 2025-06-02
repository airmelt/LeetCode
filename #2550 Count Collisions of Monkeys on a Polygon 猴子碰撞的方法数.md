# 2550 Count Collisions of Monkeys on a Polygon 猴子碰撞的方法数

__Description:__

There is a regular convex polygon with `n` vertices. The vertices are labeled from `0` to `n - 1` in a clockwise direction, and each vertex has __exactly one monkey__. The following figure shows a convex polygon of `6` vertices.

![2550-1](https://assets.leetcode.com/uploads/2023/01/22/hexagon.jpg)

Simultaneously, each monkey moves to a neighboring vertex. A collision happens if at least two monkeys reside on the same vertex after the movement or intersect on an edge.

Return the number of ways the monkeys can move so that at least __one collision__ happens. Since the answer may be very large, return it modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: n = 3
Output: 6

Explanation:

There are 8 total possible movements.
Two ways such that they collide at some point are:
```

- Monkey 1 moves in a clockwise direction; monkey 2 moves in an anticlockwise direction; monkey 3 moves in a clockwise direction. Monkeys 1 and 2 collide.
- Monkey 1 moves in an anticlockwise direction; monkey 2 moves in an anticlockwise direction; monkey 3 moves in a clockwise direction. Monkeys 1 and 3 collide.

Example 2:

```text
Input: n = 4
Output: 14
```

__Constraints:__

- `3 <= n <= 10 ^ 9`

__题目描述:__

现在有一个正凸多边形，其上共有 `n` 个顶点。顶点按顺时针方向从 `0` 到 `n - 1` 依次编号。每个顶点上 __正好有一只猴子__ 。下图中是一个 6 个顶点的凸多边形。

![2550-2](https://assets.leetcode.com/uploads/2023/01/22/hexagon.jpg)

每个猴子同时移动到相邻的顶点。顶点 `i` 的相邻顶点可以是:

- 顺时针方向的顶点 `(i + 1) % n` ，或
- 逆时针方向的顶点 `(i - 1 + n) % n` 。

如果移动后至少有两只猴子停留在同一个顶点上或者相交在一条边上，则会发生 碰撞 。

返回猴子至少发生 __一次碰撞__ 的移动方法数。由于答案可能非常大，请返回对 `10 ^ 9 + 7` 取余后的结果。

注意，每只猴子只能移动一次。

__示例:__

示例 1：

```text
输入：n = 3
输出：6
解释：共计 8 种移动方式。
下面列出两种会发生碰撞的方式：
```

- 猴子 1 顺时针移动；猴子 2 逆时针移动；猴子 3 顺时针移动。猴子 1 和猴子 2 碰撞。
- 猴子 1 逆时针移动；猴子 2 逆时针移动；猴子 3 顺时针移动。猴子 1 和猴子 3 碰撞。

可以证明，有 6 种让猴子碰撞的方法。

示例 2：

```text
输入：n = 4
输出：14
解释：可以证明，有 14 种让猴子碰撞的方法。
```

__提示：__

- `3 <= n <= 10 ^ 9`

__思路:__

```text
数学
除了全部逆时针和全部顺时针其他都会碰撞
返回 2 ^ n - 2 对 10 ^ 9 + 7 取余
可以使用快速幂加快取余
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int monkeyMove(int n) 
    {
        return (pow(2LL, n) - 2 + MOD) % MOD;
    }
private:
    const int MOD = 1e9 + 7;

    int pow(long long x, int n)
    {
        long long result = 1LL;
        for (; n; n >>= 1LL)
        {
            if (n & 1LL) result = result * x % MOD;
            x = x * x % MOD;
        }
        return result;
    }
};
```

__Java__:

```Java
import java.math.BigInteger;
class Solution {
    public int monkeyMove(int n) {
        return (BigInteger.TWO.modPow(BigInteger.valueOf(n), BigInteger.valueOf(1_000_000_007)).intValue() + 1_000_000_005) % 1_000_000_007;
    }
}
```

__Python__:

```Python
class Solution:
    def monkeyMove(self, n: int) -> int:
        return (pow(2, n, 10 ** 9 + 7) + 10 ** 9 + 5) % (10 ** 9 + 7)
```
