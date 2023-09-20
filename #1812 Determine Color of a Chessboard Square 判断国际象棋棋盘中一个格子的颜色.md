# 1812 Determine Color of a Chessboard Square 判断国际象棋棋盘中一个格子的颜色

__Description:__

You are given `coordinates`, a string that represents the coordinates of a square of the chessboard. Below is a chessboard for your reference.

![1812-1](https://assets.leetcode.com/uploads/2021/02/19/screenshot-2021-02-20-at-22159-pm.png)

Return `true` _if the square is white, and_ `false` _if the square is black_.

The coordinate will always represent a valid chessboard square. The coordinate will always have the letter first, and the number second.

__Example:__

Example 1:

```text
Input: coordinates = "a1"
Output: false
Explanation: From the chessboard above, the square with coordinates "a1" is black, so return false.
```

Example 2:

```text
Input: coordinates = "h3"
Output: true
Explanation: From the chessboard above, the square with coordinates "h3" is white, so return true.
```

Example 3:

```text
Input: coordinates = "c7"
Output: false
```

__Constraints:__

- `coordinates.length == 2`
- `'a' <= coordinates[0] <= 'h'`
- `'1' <= coordinates[1] <= '8'`

__题目描述:__

给你一个坐标 `coordinates` ，它是一个字符串，表示国际象棋棋盘中一个格子的坐标。下图是国际象棋棋盘示意图。

![1812-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/03/chessboard.png)

如果所给格子的颜色是白色，请你返回 `true`，如果是黑色，请返回 `false` 。

给定坐标一定代表国际象棋棋盘上一个存在的格子。坐标第一个字符是字母，第二个字符是数字。

__示例:__

示例 1：

```text
输入：coordinates = "a1"
输出：false
解释：如上图棋盘所示，"a1" 坐标的格子是黑色的，所以返回 false 。
```

示例 2：

```text
输入：coordinates = "h3"
输出：true
解释：如上图棋盘所示，"h3" 坐标的格子是白色的，所以返回 true 。
```

示例 3：

```text
输入：coordinates = "c7"
输出：false
```

__提示：__

- `coordinates.length == 2`
- `'a' <= coordinates[0] <= 'h'`
- `'1' <= coordinates[1] <= '8'`

__思路:__

```text
位运算
找规律
求第一个字符减去 'a' 的 ASCII 值
和第二个字符减去 '0' 的 ASCII 值
两者取异或的值要等于 0
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool squareIsWhite(string coordinates) 
    {
        return (coordinates[0] ^ coordinates[1]) & 1;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean squareIsWhite(String coordinates) {
        return (((coordinates.charAt(0) - 'a') ^ (coordinates.charAt(1) - '0')) & 1) == 0;
    }
}
```

__Python__:

```Python
class Solution:
    def squareIsWhite(self, coordinates: str) -> bool:
        return not ((ord(coordinates[0]) - ord('a') & 1) + (int(coordinates[1]) & 1)) & 1
```
