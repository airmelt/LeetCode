# 2543 Check if Point Is Reachable 判断一个点是否可以到达

__Description:__

There exists an infinitely large grid. You are currently at point `(1, 1)`, and you need to reach the point `(targetX, targetY)` using a finite number of steps.

In one __step__, you can move from point `(x, y)` to any one of the following points:

- `(x, y - x)`
- `(x - y, y)`
- `(2 * x, y)`
- `(x, 2 * y)`

Given two integers `targetX` and `targetY` representing the X-coordinate and Y-coordinate of your final position, return `true` _if you can reach the point from_ `(1, 1)` _using some number of steps, and_ `false` _otherwise_.

__Example:__

Example 1:

```text
Input: targetX = 6, targetY = 9
Output: false
Explanation: It is impossible to reach (6,9) from (1,1) using any sequence of moves, so false is returned.
```

Example 2:

```text
Input: targetX = 4, targetY = 7
Output: true
Explanation: You can follow the path (1,1) -> (1,2) -> (1,4) -> (1,8) -> (1,7) -> (2,7) -> (4,7).
```

__Constraints:__

- `1 <= targetX, targetY <= 10 ^ 9`

__题目描述:__

给你一个无穷大的网格图。一开始你在 `(1, 1)` ，你需要通过有限步移动到达点 `(targetX, targetY)` 。

_每一步_ ，你可以从点 `(x, y)` 移动到以下点之一:

- `(x, y - x)`
- `(x - y, y)`
- `(2 * x, y)`
- `(x, 2 * y)`

给你两个整数 `targetX` 和 `targetY` ，分别表示你最后需要到达点的 X 和 Y 坐标。如果你可以从 `(1, 1)` 出发到达这个点，请你返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：targetX = 6, targetY = 9
输出：false
解释：没法从 (1,1) 出发到达 (6,9) ，所以返回 false 。
```

示例 2：

```text
输入：targetX = 4, targetY = 7
输出：true
解释：你可以按照以下路径到达：(1,1) -> (1,2) -> (1,4) -> (1,8) -> (1,7) -> (2,7) -> (4,7) 。
```

__提示：__

- `1 <= targetX, targetY <= 10 ^ 9`

__思路:__

```text
数学
前两个操作很像辗转相除法
考虑两个数的最大公约数 g
如果 g 是 2 的幂次方, 那么可以通过上述操作到达 (targetX, targetY)
2 的幂次方可以用 g & (g - 1) == 0 来判断
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isReachable(int targetX, int targetY) 
    {
        return !((targetX = gcd(targetX, targetY)) & (targetX - 1));
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isReachable(int targetX, int targetY) {
        int g = gcd(targetX, targetY);
        return (g & (g - 1)) == 0;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def isReachable(self, targetX: int, targetY: int) -> bool:
        return not (g := gcd(targetX, targetY)) & (g - 1)
```
