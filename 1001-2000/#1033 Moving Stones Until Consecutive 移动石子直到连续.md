# 1033 Moving Stones Until Consecutive 移动石子直到连续

__Description__:
Three stones are on a number line at positions a, b, and c.

Each turn, you pick up a stone at an endpoint (ie., either the lowest or highest position stone), and move it to an unoccupied position between those endpoints.  Formally, let's say the stones are currently at positions x, y, z with x < y < z.  You pick up the stone at either position x or position z, and move that stone to an integer position k, with x < k < z and k != y.

The game ends when you cannot make any more moves, ie. the stones are in consecutive positions.

When the game ends, what is the minimum and maximum number of moves that you could have made?  Return the answer as an length 2 array: answer = [minimum_moves, maximum_moves]

__Example:__

Example 1:

Input: a = 1, b = 2, c = 5
Output: [1,2]
Explanation: Move the stone from 5 to 3, or move the stone from 5 to 4 to 3.

Example 2:

Input: a = 4, b = 3, c = 2
Output: [0,0]
Explanation: We cannot make any moves.

Example 3:

Input: a = 3, b = 5, c = 1
Output: [1,2]
Explanation: Move the stone from 1 to 4; or move the stone from 1 to 2 to 4.

__Note:__

1 <= a <= 100
1 <= b <= 100
1 <= c <= 100
a != b, b != c, c != a

__题目描述__:
三枚石子放置在数轴上，位置分别为 a，b，c。

每一回合，我们假设这三枚石子当前分别位于位置 x, y, z 且 x < y < z。从位置 x 或者是位置 z 拿起一枚石子，并将该石子移动到某一整数位置 k 处，其中 x < k < z 且 k != y。

当你无法进行任何移动时，即，这些石子的位置连续时，游戏结束。

要使游戏结束，你可以执行的最小和最大移动次数分别是多少？ 以长度为 2 的数组形式返回答案：answer = [minimum_moves, maximum_moves]

__示例 :__

示例 1：

输入：a = 1, b = 2, c = 5
输出：[1, 2]
解释：将石子从 5 移动到 4 再移动到 3，或者我们可以直接将石子移动到 3。

示例 2：

输入：a = 4, b = 3, c = 2
输出：[0, 0]
解释：我们无法进行任何移动。

__提示：__

1 <= a <= 100
1 <= b <= 100
1 <= c <= 100
a != b, b != c, c != a

__思路__:

先按大小顺序排序a, b, c, 如果三个数连续, 不需要移动, 输出{0, 0}; 如果相邻两个数相差为 1或者 2, 则将第三个数放在后面或者插入中间, 输出{1, max - min + 2}; 否则输出{2, max - min + 2}, max - min + 2表示最大和最小值中间的间隔值
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> numMovesStones(int a, int b, int c) 
    {
        int x = min(min(a, b), c), z = max(max(a, b), c);
        int y = a + b + c - x - z;
        if (y - x == 1 && z - y == 1) return {0, 0};
        else if (y - x < 3 || z - y < 3) return {1, z - x - 2};
        else return {2, z - x - 2};
    }
};
```

__Java__:

```Java
class Solution {
    public int[] numMovesStones(int a, int b, int c) {
        int x = Math.min(Math.min(a, b), c), z = Math.max(Math.max(a, b), c);
        int y = a + b + c - x - z;
        if (y - x == 1 && z - y == 1) return new int[]{0, 0};
        else if (y - x < 3 || z - y < 3) return new int[]{1, z - x - 2};
        else return new int[]{2, z - x - 2};
    }
}
```

__Python__:

```Python
class Solution:
    def numMovesStones(self, a: int, b: int, c: int) -> List[int]:
        return [1 if ((a + b + c - max(a, b, c) - min(a, b, c) * 2 == 2) or (2 * max(a, b, c) - a - b - c + min(a, b, c) == 2)) else (a + b + c - max(a, b, c) - min(a, b, c) * 2 != 1) + (2 * max(a, b, c) - a - b - c + min(a, b, c) != 1), max(a, b, c) - min(a, b, c) - 2]
```
