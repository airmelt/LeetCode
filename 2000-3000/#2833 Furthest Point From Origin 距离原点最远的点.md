# 2833 Furthest Point From Origin 距离原点最远的点

__Description:__

You are given a string `moves` of length `n` consisting only of characters `'L'`, `'R'`, and `'_'`. The string represents your movement on a number line starting from the origin `0`.

In the `i ^ th` move, you can choose one of the following directions:

- move to the left if `moves[i] = 'L'` or `moves[i] = '_'`
- move to the right if `moves[i] = 'R'` or `moves[i] = '_'`

Return _the __distance from the origin__ of the __furthest__ point you can get to after_ `n` _moves_.

__Example:__

Example 1:

```text
Input: moves = "L_RLR"
Output: 3
Explanation: The furthest point we can reach from the origin 0 is point -3 through the following sequence of moves "LLRLLLR".
```

Example 2:

```text
Input: moves = "_RLL_"
Output: 5
Explanation: The furthest point we can reach from the origin 0 is point -5 through the following sequence of moves "LRLLLLL".
```

Example 3:

```text
Input: moves = "_"
Output: 7
Explanation: The furthest point we can reach from the origin 0 is point 7 through the following sequence of moves "RRRRRRR".
```

__Constraints:__

- `1 <= moves.length == n <= 50`
- `moves` consists only of characters `'L'`, `'R'` and `'_'`.

__题目描述:__

给你一个长度为 `n` 的字符串 `moves` ，该字符串仅由字符 `'L'`、 `'R'` 和 `'_'` 组成。字符串表示你在一条原点为 `0` 的数轴上的若干次移动。

你的初始位置就在原点（ `0`），第 `i` 次移动过程中，你可以根据对应字符选择移动方向:

- 如果 `moves[i] = 'L'` 或 `moves[i] = '_'` ，可以选择向左移动一个单位距离
- 如果 `moves[i] = 'R'` 或 `moves[i] = '_'` ，可以选择向右移动一个单位距离

移动 `n` 次之后，请你找出可以到达的距离原点 __最远__ 的点，并返回 __从原点到这一点的距离__ 。

__示例:__

示例 1：

```text
输入：moves = "L_RLR"
输出：3
解释：可以到达的距离原点 0 最远的点是 -3 ，移动的序列为 "LLRLLLR" 。
```

示例 2：

```text
输入：moves = "_RLL_"
输出：5
解释：可以到达的距离原点 0 最远的点是 -5 ，移动的序列为 "LRLLLLL" 。
```

示例 3：

```text
输入：moves = "_"
输出：7
解释：可以到达的距离原点 0 最远的点是 7 ，移动的序列为 "RRRRRRR" 。
```

__提示：__

- `1 <= moves.length == n <= 50`
- `moves` 仅由字符 `'L'`、 `'R'` 和 `'_'` 组成

__思路:__

```text
脑筋急转弯
要么把下划线全部换成 L
要么全部换成 R
所以只需要比较 L 和 R 的差值的绝对值
再加上下划线的数量即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int furthestDistanceFromOrigin(string moves) 
    {
        return abs(count(moves.begin(), moves.end(), 'L') - count(moves.begin(), moves.end(), 'R')) + count(moves.begin(), moves.end(), '_');
    }
};
```

__Java__:

```Java
class Solution {
    public int furthestDistanceFromOrigin(String moves) {
        return (int)(Math.abs(moves.chars().filter(c -> c == 'L').count() - moves.chars().filter(c -> c == 'R').count()) + moves.chars().filter(c -> c == '_').count());
    }
}
```

__Python__:

```Python
class Solution:
    def furthestDistanceFromOrigin(self, moves: str) -> int:
        return abs(moves.count("L") - moves.count("R")) + moves.count("_")
```
