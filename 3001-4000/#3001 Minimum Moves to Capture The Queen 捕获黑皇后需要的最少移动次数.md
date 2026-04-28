# 3001 Minimum Moves to Capture The Queen 捕获黑皇后需要的最少移动次数

__Description:__

There is a __1-indexed__ `8 x 8` chessboard containing `3` pieces.

You are given `6` integers `a`, `b`, `c`, `d`, `e`, and `f` where:

- `(a, b)` denotes the position of the white rook.
- `(c, d)` denotes the position of the white bishop.
- `(e, f)` denotes the position of the black queen.

Given that you can only move the white pieces, return the minimum number of moves required to capture the black queen.

Note that:

- Rooks can move any number of squares either vertically or horizontally, but cannot jump over other pieces.
- Bishops can move any number of squares diagonally, but cannot jump over other pieces.
- A rook or a bishop can capture the queen if it is located in a square that they can move to.
- The queen does not move.

__Example:__

Example 1:

![3001-1](https://assets.leetcode.com/uploads/2023/12/21/ex1.png)

```text
Input: a = 1, b = 1, c = 8, d = 8, e = 2, f = 3
Output: 2
Explanation: We can capture the black queen in two moves by moving the white rook to (1, 3) then to (2, 3).
It is impossible to capture the black queen in less than two moves since it is not being attacked by any of the pieces at the beginning.
```

Example 2:

![3001-2](https://assets.leetcode.com/uploads/2023/12/21/ex2.png)

```text
Input: a = 5, b = 3, c = 3, d = 4, e = 5, f = 2
Output: 1
Explanation: We can capture the black queen in a single move by doing one of the following: 
- Move the white rook to (5, 2).
- Move the white bishop to (5, 2).
```

__Constraints:__

- `1 <= a, b, c, d, e, f <= 8`
- No two pieces are on the same square.

__题目描述:__

现有一个下标从 __1__ 开始的 `8 x 8` 棋盘，上面有 `3` 枚棋子。

给你 `6` 个整数 `a` 、 `b` 、 `c` 、 `d` 、 `e` 和 `f` ，其中:

- `(a, b)` 表示白色车的位置。
- `(c, d)` 表示白色象的位置。
- `(e, f)` 表示黑皇后的位置。

假定你只能移动白色棋子，返回捕获黑皇后所需的最少移动次数。

请注意：

- 车可以向垂直或水平方向移动任意数量的格子，但不能跳过其他棋子。
- 象可以沿对角线方向移动任意数量的格子，但不能跳过其他棋子。
- 如果车或象能移向皇后所在的格子，则认为它们可以捕获皇后。
- 皇后不能移动。

__示例:__

示例 1：

![3001-3](https://assets.leetcode.com/uploads/2023/12/21/ex1.png)

```text
输入：a = 1, b = 1, c = 8, d = 8, e = 2, f = 3
输出：2
解释：将白色车先移动到 (1, 3) ，然后移动到 (2, 3) 来捕获黑皇后，共需移动 2 次。
由于起始时没有任何棋子正在攻击黑皇后，要想捕获黑皇后，移动次数不可能少于 2 次。
```

示例 2：

![3001-4](https://assets.leetcode.com/uploads/2023/12/21/ex2.png)

```text
输入：a = 5, b = 3, c = 3, d = 4, e = 5, f = 2
输出：1
解释：可以通过以下任一方式移动 1 次捕获黑皇后：
- 将白色车移动到 (5, 2) 。
- 将白色象移动到 (5, 2) 。
```

__提示：__

- `1 <= a, b, c, d, e, f <= 8`
- 两枚棋子不会同时出现在同一个格子上。

__思路:__

```text
分类讨论
无论如何 2 步之内车都能抓到后
只需要讨论如何能够一步杀
1. 车和后在一条线上要么 a == e 要么 b == f, 此时需要保证在车和后的中间没有象
下面讨论 a == e, b == f 同理
1.1 要么象不在这条线上, c != e
1.2 要么象不在车和后的中间, (d - b) * (d - f) > 0
2. 象和后在一条斜线上要么 c + d = e + f, 要么 c - d = e - f
下面讨论 c + d = e + f, c - d = e - f 同理
当 c + d = e + f 时说明象和后在同一条副对角线上
2.1 要么车不在这条副对角线上, a + b != e + f
2.2 要么车不在象和后的中间, (a - c) * (a - e) > 0
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minMovesToCaptureTheQueen(int a, int b, int c, int d, int e, int f) 
    {
        return 2 - (a == e and (c != e or (d - b) * (d - f) > 0) or b == f and (d != f or (c - a) * (c - e) > 0) or c + d == e + f and (a + b != e + f or (a - c) * (a - e) > 0) or c - d == e - f and (a - b != e - f or (a - c) * (a - e) > 0));
    }
};
```

__Java__:

```Java
class Solution {
    public int minMovesToCaptureTheQueen(int a, int b, int c, int d, int e, int f) {
        return a == e && (c != e || (d - b) * (d - f) > 0) || b == f && (d != f || (c - a) * (c - e) > 0) || c + d == e + f && (a + b != e + f || (a - c) * (a - e) > 0) || c - d == e - f && (a - b != e - f || (a - c) * (a - e) > 0) ? 1 : 2;
    }
}
```

__Python__:

```Python
class Solution:
    def minMovesToCaptureTheQueen(self, a: int, b: int, c: int, d: int, e: int, f: int) -> int:
        return 1 if a == e and (c != e or (d - b) * (d - f) > 0) or b == f and (d != f or (c - a) * (c - e) > 0) or c + d == e + f and (a + b != e + f or (a - c) * (a - e) > 0) or c - d == e - f and (a - b != e - f or (a - c) * (a - e) > 0) else 2
```
