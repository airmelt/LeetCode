# 790 Domino and Tromino Tiling 多米诺和托米诺平铺

__Description__:
You have two types of tiles: a 2 x 1 domino shape and a tromino shape. You may rotate these shapes.

![tile](https://assets.leetcode.com/uploads/2021/07/15/lc-domino.jpg)

Given an integer n, return the number of ways to tile an 2 x n board. Since the answer may be very large, return it modulo 10^9 + 7.

In a tiling, every square must be covered by a tile. Two tilings are different if and only if there are two 4-directionally adjacent cells on the board such that exactly one of the tilings has both squares occupied by a tile.

__Example:__

Example 1:

![domino](https://assets.leetcode.com/uploads/2021/07/15/lc-domino1.jpg)

Input: n = 3
Output: 5
Explanation: The five different ways are show above.

Example 2:

Input: n = 1
Output: 1

__Constraints:__

1 <= n <= 1000

__题目描述__:
有两种形状的瓷砖：一种是 2x1 的多米诺形，另一种是形如 "L" 的托米诺形。两种形状都可以旋转。

```text
XX  <- 多米诺

XX  <- "L" 托米诺
X
```

给定 N 的值，有多少种方法可以平铺 2 x N 的面板？返回值 mod 10^9 + 7。

（平铺指的是每个正方形都必须有瓷砖覆盖。两个平铺不同，当且仅当面板上有四个方向上的相邻单元中的两个，使得恰好有一个平铺有一个瓷砖占据两个正方形。）

__示例 :__

输入: 3
输出: 5
解释:
下面列出了五种不同的方法，不同字母代表不同瓷砖：

```text
XYZ XXZ XYY XXY XYY
XYZ YYZ XZZ XYY XXY
```

__提示:__

N  的范围是 [1, 1000]

__思路__:

动态规划
先穷举出 n <= 4 的所有情况找规律

```text
    a
    a
n = 1 -> 1
    ab     aa
    ab     bb
n = 2 -> 2
    abc    abb    aac    aab    abb    
    abc    acc    bbc    abb    aab
n = 3 -> 5
    abcd   abcc   abbd   abbc   abcc   aacd   aacc   abbc   aabc   abbc   aacc
    abcd   abdd   accd   abcc   abbc   bbcd   bbdd   aabc   abbc   aacc   abbc
n = 4 -> 11
```

设 f(n) 表示 n 对应的摆法数目
可以观察到 n = 4, 实际上由 n = 3 及 n = 2, n = 1, 还有两个特殊摆法 (f(0)) 组成
令 f(0) = 1
f(4) = f(3) + f(2) + 2 \* (f(1) + f(0))
f(5) = f(4) + f(3) + 2 \* (f(2) + f(1) + f(0))
f(n) = f(n - 1) + f(n - 2) + 2 \* (f(n - 3) + ... + f(0))
f(n - 1) = f(n - 2) + f(n - 3) + 2 \* (f(n - 4) + ... + f(0))
f(n) - f(n - 1) = f(n - 1) + f(n - 3)
f(n) = f(n - 3) + 2 \* f(n - 1)
可以用动态规划, 由于 f(n) 只和前面 3 项相关可以将空间复杂度压缩到 1
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numTilings(int n) 
    {
        int a = 1, b = 1, c = 2, m = 1000000007, cur = 0;
        for (int i = 3; i <= n; i++) 
        {
            cur = ((c << 1) % m + a % m) % m;
            a = b;
            b = c;
            c = cur;
        }
        return n > 1 ? c : a;
    }
};
```

__Java__:

```Java
class Solution {
    public int numTilings(int n) {
        int a = 1, b = 1, c = 2, m = 1000000007, cur = 0;
        for (int i = 3; i <= n; i++) {
            cur = ((c << 1) % m + a % m) % m;
            a = b;
            b = c;
            c = cur;
        }
        return n > 1 ? c : a;
    }
}
```

__Python__:

```Python
class Solution:
    def numTilings(self, n: int) -> int:
        a, b, c = 1, 1, 2
        for i in range(3, n + 1):
            a, b, c = b, c, (c << 1) + a
        return c % 1000000007 if n > 1 else a
```
