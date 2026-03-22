# 1861 Rotating the Box 旋转盒子

__Description:__

You are given an `m x n` matrix of characters `box` representing a side-view of a box. Each cell of the box is one of the following:

- A stone `'#'`
- A stationary obstacle `'*'`
- Empty `'.'`

The box is rotated 90 degrees clockwise, causing some of the stones to fall due to gravity. Each stone falls down until it lands on an obstacle, another stone, or the bottom of the box. Gravity does not affect the obstacles' positions, and the inertia from the box's rotation does not affect the stones' horizontal positions.

It is __guaranteed__ that each stone in `box` rests on an obstacle, another stone, or the bottom of the box.

Return _an_ `n x m` _matrix representing the box after the rotation described above_.

__Example:__

Example 1:

![1861-1](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcodewithstones.png)

```text
Input: box = [["#",".","#"]]
Output: [["."],
         ["#"],
         ["#"]]
```

Example 2:

![1861-2](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode2withstones.png)

```text
Input: box = [["#",".","*","."],
              ["#","#","*","."]]
Output: [["#","."],
         ["#","#"],
         ["*","*"],
         [".","."]]
```

Example 3:

![1861-3](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode3withstone.png)

```text
Input: box = [["#","#","*",".","*","."],
              ["#","#","#","*",".","."],
              ["#","#","#",".","#","."]]
Output: [[".","#","#"],
         [".","#","#"],
         ["#","#","*"],
         ["#","*","."],
         ["#",".","*"],
         ["#",".","."]]
```

__Constraints:__

- `m == box.length`
- `n == box[i].length`
- `1 <= m, n <= 500`
- `box[i][j]` is either `'#'`, `'*'`, or `'.'`.

__题目描述:__

给你一个 `m x n` 的字符矩阵 `box` ，它表示一个箱子的侧视图。箱子的每一个格子可能为:

- `'#'` 表示石头
- `'*'` 表示固定的障碍物
- `'.'` 表示空位置

这个箱子被 顺时针旋转 90 度 ，由于重力原因，部分石头的位置会发生改变。每个石头会垂直掉落，直到它遇到障碍物，另一个石头或者箱子的底部。重力 不会 影响障碍物的位置，同时箱子旋转不会产生惯性 ，也就是说石头的水平位置不会发生改变。

题目保证初始时 `box` 中的石头要么在一个障碍物上，要么在另一个石头上，要么在箱子的底部。

请你返回一个 `n x m`的矩阵，表示按照上述旋转后，箱子内的结果。

__示例:__

示例 1：

![1861-4](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcodewithstones.png)

```text
输入：box = [["#",".","#"]]
输出：[["."],
      ["#"],
      ["#"]]
```

示例 2：

![1861-5](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode2withstones.png)

```text
输入：box = [["#",".","*","."],
            ["#","#","*","."]]
输出：[["#","."],
      ["#","#"],
      ["*","*"],
      [".","."]]
```

示例 3：

![1861-6](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode3withstone.png)

```text
输入：box = [["#","#","*",".","*","."],
            ["#","#","#","*",".","."],
            ["#","#","#",".","#","."]]
输出：[[".","#","#"],
      [".","#","#"],
      ["#","#","*"],
      ["#","*","."],
      ["#",".","*"],
      ["#",".","."]]
```

__提示：__

- `m == box.length`
- `n == box[i].length`
- `1 <= m, n <= 500`
- `box[i][j]` 只可能是 `'#'` ， `'*'` 或者 `'.'` 。

__思路:__

```text
模拟
先旋转 90 度
从下往上依次将空位置替换为石头
如果碰到障碍物, 将下一次放石头的位置改为障碍物的上一个位置
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& box)
     {
        int m = box.size(), n = box.front().size();
        vector<vector<char>> result(n, vector<char>(m));
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result[j][m - i - 1] = box[i][j];
        for (int i = 0, start = n - 1; i < m; i++, start = n - 1) 
        {
            for (int j = start; j > -1; j--) 
            {
                if (result[j][i] == '*') start = j - 1;
                else if (result[j][i] == '#') 
                {
                    result[j][i] = '.';
                    result[start--][i] = '#';
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public char[][] rotateTheBox(char[][] box) {
        int m = box.length, n = box[0].length;
        char[][] result = new char[n][m];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result[j][m - i - 1] = box[i][j];
        for (int i = 0, start = n - 1; i < m; i++, start = n - 1) {
            for (int j = start; j > -1; j--) {
                if (result[j][i] == '*') start = j - 1;
                else if (result[j][i] == '#') {
                    result[j][i] = '.';
                    result[start--][i] = '#';
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def rotateTheBox(self, box: List[List[str]]) -> List[List[str]]:
        m, n = len(box), len(box[0])
        result = [[''] * m for _ in range(n)]
        for i in range(m):
            for j in range(n):
                result[j][m - i - 1] = box[i][j]
        for i in range(m):
            start = n - 1
            for j in range(n - 1, -1, -1):
                if result[j][i] == '*':
                    start = j - 1
                elif result[j][i] == '#':
                    result[j][i], result[start][i] = '.', '#'
                    start -= 1
        return result
```
