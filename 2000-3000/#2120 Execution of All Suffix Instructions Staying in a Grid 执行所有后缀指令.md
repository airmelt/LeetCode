# 2120 Execution of All Suffix Instructions Staying in a Grid 执行所有后缀指令

__Description:__

There is an `n x n` grid, with the top-left cell at `(0, 0)` and the bottom-right cell at `(n - 1, n - 1)`. You are given the integer `n` and an integer array `startPos` where `startPos = [startrow, startcol]` indicates that a robot is initially at cell `(startrow, startcol)`.

You are also given a __0-indexed__ string `s` of length `m` where `s[i]` is the `i ^ th` instruction for the robot: `'L'` (move left), `'R'` (move right), `'U'` (move up), and `'D'` (move down).

The robot can begin executing from any `i ^ th` instruction in `s`. It executes the instructions one by one towards the end of `s` but it stops if either of these conditions is met:

- The next instruction will move the robot off the grid.
- There are no more instructions left to execute.

Return _an array_ `answer` _of length_ `m` _where_ `answer[i]` _is __the number of instructions__ the robot can execute if the robot __begins executing from__ the_ `i ^ th` _instruction in_ `s`.

__Example:__

Example 1:

![2120-1](https://assets.leetcode.com/uploads/2021/12/09/1.png)

```text
Input: n = 3, startPos = [0,1], s = "RRDDLU"
Output: [1,5,4,3,1,0]
Explanation: Starting from startPos and beginning execution from the ith instruction:
- 0th: "RRDDLU". Only one instruction "R" can be executed before it moves off the grid.
- 1st:  "RDDLU". All five instructions can be executed while it stays in the grid and ends at (1, 1).
- 2nd:   "DDLU". All four instructions can be executed while it stays in the grid and ends at (1, 0).
- 3rd:    "DLU". All three instructions can be executed while it stays in the grid and ends at (0, 0).
- 4th:     "LU". Only one instruction "L" can be executed before it moves off the grid.
- 5th:      "U". If moving up, it would move off the grid.
```

Example 2:

![2120-2](https://assets.leetcode.com/uploads/2021/12/09/2.png)

```text
Input: n = 2, startPos = [1,1], s = "LURD"
Output: [4,1,0,0]
Explanation:
- 0th: "LURD".
- 1st:  "URD".
- 2nd:   "RD".
- 3rd:    "D".
```

Example 3:

![2120-3](https://assets.leetcode.com/uploads/2021/12/09/3.png)

```text
Input: n = 1, startPos = [0,0], s = "LRUD"
Output: [0,0,0,0]
Explanation: No matter which instruction the robot begins execution from, it would move off the grid.
```

__Constraints:__

- `m == s.length`
- `1 <= n, m <= 500`
- `startPos.length == 2`
- `0 <= startrow, startcol < n`
- `s` consists of `'L'`, `'R'`, `'U'`, and `'D'`.

__题目描述:__

现有一个 `n x n` 大小的网格，左上角单元格坐标 `(0, 0)` ，右下角单元格坐标 `(n - 1, n - 1)` 。给你整数 `n` 和一个整数数组 `startPos` ，其中 `startPos = [startrow, startcol]` 表示机器人最开始在坐标为 `(startrow, startcol)` 的单元格上。

另给你一个长度为 `m` 、下标从 __0__ 开始的字符串 `s` ，其中 `s[i]` 是对机器人的第 `i` 条指令: `'L'`（向左移动）， `'R'`（向右移动）， `'U'`（向上移动）和 `'D'`（向下移动）。

机器人可以从 `s` 中的任一第 `i` 条指令开始执行。它将会逐条执行指令直到 `s` 的末尾，但在满足下述条件之一时，机器人将会停止:

- 下一条指令将会导致机器人移动到网格外。
- 没有指令可以执行。

返回一个长度为 `m` 的数组 `answer` ，其中 `answer[i]` 是机器人从第 `i` 条指令 __开始__ ，可以执行的 __指令数目__ 。

__示例:__

示例 1：

![2120-4](https://assets.leetcode.com/uploads/2021/12/09/1.png)

```text
输入：n = 3, startPos = [0,1], s = "RRDDLU"
输出：[1,5,4,3,1,0]
解释：机器人从 startPos 出发，并从第 i 条指令开始执行：
- 0: "RRDDLU" 在移动到网格外之前，只能执行一条 "R" 指令。
- 1:  "RDDLU" 可以执行全部五条指令，机器人仍在网格内，最终到达 (0, 0) 。
- 2:   "DDLU" 可以执行全部四条指令，机器人仍在网格内，最终到达 (0, 0) 。
- 3:    "DLU" 可以执行全部三条指令，机器人仍在网格内，最终到达 (0, 0) 。
- 4:     "LU" 在移动到网格外之前，只能执行一条 "L" 指令。
- 5:      "U" 如果向上移动，将会移动到网格外。
```

示例 2：

![2120-5](https://assets.leetcode.com/uploads/2021/12/09/2.png)

```text
输入：n = 2, startPos = [1,1], s = "LURD"
输出：[4,1,0,0]
解释：
- 0: "LURD"
- 1:  "URD"
- 2:   "RD"
- 3:    "D"
```

示例 3：

![2120-6](https://assets.leetcode.com/uploads/2021/12/09/3.png)

```text
输入：n = 1, startPos = [0,0], s = "LRUD"
输出：[0,0,0,0]
解释：无论机器人从哪条指令开始执行，都会移动到网格外。
```

__提示：__

- `m == s.length`
- `1 <= n, m <= 500`
- `startPos.length == 2`
- `0 <= startrow, startcol < n`
- `s` 由 `'L'`、 `'R'`、 `'U'` 和 `'D'` 组成

__思路:__

```text
模拟
每次从 i 开始
模拟机器人的路径
如果机器人超出边界, 就直接退出遍历
时间复杂度为 O(M ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> executeInstructions(int n, vector<int>& startPos, string s) 
    {
        vector<int> result;
        for (int i = 0, m = s.size(), x = startPos.front(), y = startPos.back(), step = m - i; i < m; i++, x = startPos.front(), y = startPos.back(), step = m - i) 
        {
            for (int j = i; j < m; j++) 
            {
                x += (s[j] == 'D') - (s[j] == 'U');
                y += (s[j] == 'R') - (s[j] == 'L');
                if (x < 0 or x >= n or y < 0 or y >= n) 
                {
                    step = j - i;
                    break;
                }
            }
            result.emplace_back(step);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] executeInstructions(int n, int[] startPos, String s) {
        int m = s.length(), result[] = new int[m];
        for (int i = 0, x = startPos[0], y = startPos[1], step = m - i; i < m; i++, x = startPos[0], y = startPos[1], step = m - i) {
            for (int j = i; j < m; j++) {
                x += (s.charAt(j) == 'D' ? 1 : 0) + (s.charAt(j) == 'U' ? -1 : 0);
                y += (s.charAt(j) == 'R' ? 1 : 0) + (s.charAt(j) == 'L' ? -1 : 0);
                if (x < 0 || x >= n || y < 0 || y >= n) {
                    step = j - i;
                    break;
                }
            }
            result[i] = step;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def executeInstructions(self, n: int, startPos: List[int], s: str) -> List[int]:
        d, result = { 'D': (1, 0), 'U': (-1, 0), 'L': (0, -1), 'R': (0, 1) }, [0] * (m := len(s))
        for i in range(m):
            cur, r = startPos[:], 0
            for j in range(i, m):
                if cur[0] + d[s[j]][0] < 0 or cur[0] + d[s[j]][0] >= n:
                    break
                if cur[1] + d[s[j]][1] < 0 or cur[1] + d[s[j]][1] >= n:
                    break
                cur[0] += d[s[j]][0]
                cur[1] += d[s[j]][1]
                r += 1
            result[i] = r
        return result
```
