# 2056 Number of Valid Move Combinations On Chessboard 棋盘上有效移动组合的数目

__Description:__

There is an `8 x 8` chessboard containing `n` pieces (rooks, queens, or bishops). You are given a string array `pieces` of length `n`, where `pieces[i]` describes the type (rook, queen, or bishop) of the `i ^ th` piece. In addition, you are given a 2D integer array `positions` also of length `n`, where `positions[i] = [ri, ci]` indicates that the `i ^ th` piece is currently at the __1-based__ coordinate `(ri, ci)` on the chessboard.

When making a move for a piece, you choose a destination square that the piece will travel toward and stop on.

- A rook can only travel __horizontally or vertically__ from `(r, c)` to the direction of `(r+1, c)`, `(r-1, c)`, `(r, c+1)`, or `(r, c-1)`.
- A queen can only travel __horizontally, vertically, or diagonally__ from `(r, c)` to the direction of `(r+1, c)`, `(r-1, c)`, `(r, c+1)`, `(r, c-1)`, `(r+1, c+1)`, `(r+1, c-1)`, `(r-1, c+1)`, `(r-1, c-1)`.
- A bishop can only travel __diagonally__ from `(r, c)` to the direction of `(r+1, c+1)`, `(r+1, c-1)`, `(r-1, c+1)`, `(r-1, c-1)`.

You must make a __move__ for every piece on the board simultaneously. A __move combination__ consists of all the __moves__ performed on all the given pieces. Every second, each piece will instantaneously travel __one square__ towards their destination if they are not already at it. All pieces start traveling at the `0 ^ th` second. A move combination is __invalid__ if, at a given time, __two or more__ pieces occupy the same square.

Return the number of valid move combinations.

Notes:

- __No two pieces__ will start in the __same__ square.
- You may choose the square a piece is already on as its __destination__.
- If two pieces are __directly adjacent__ to each other, it is valid for them to __move past each other__ and swap positions in one second.

__Example:__

Example 1:

![2056-1](https://assets.leetcode.com/uploads/2021/09/23/a1.png)

```text
Input: pieces = ["rook"], positions = [[1,1]]
Output: 15
Explanation: The image above shows the possible squares the piece can move to.
```

Example 2:

![2056-2](https://assets.leetcode.com/uploads/2021/09/23/a2.png)

```text
Input: pieces = ["queen"], positions = [[1,1]]
Output: 22
Explanation: The image above shows the possible squares the piece can move to.
```

Example 3:

![2056-3](https://assets.leetcode.com/uploads/2021/09/23/a3.png)

```text
Input: pieces = ["bishop"], positions = [[4,3]]
Output: 12
Explanation: The image above shows the possible squares the piece can move to.
```

__Constraints:__

- `n == pieces.length`
- `n == positions.length`
- `1 <= n <= 4`
- `pieces` only contains the strings `"rook"`, `"queen"`, and `"bishop"`.
- There will be at most one queen on the chessboard.
- `1 <= xi, yi <= 8`
- Each `positions[i]` is distinct.

__题目描述:__

有一个 `8 x 8` 的棋盘，它包含 `n` 个棋子（棋子包括车，后和象三种）。给你一个长度为 `n` 的字符串数组 `pieces` ，其中 `pieces[i]` 表示第 `i` 个棋子的类型（车，后或象）。除此以外，还给你一个长度为 `n` 的二维整数数组 `positions` ，其中 `positions[i] = [ri, ci]` 表示第 `i` 个棋子现在在棋盘上的位置为 `(ri, ci)` ，棋盘下标从 __1__ 开始。

棋盘上每个棋子都可以移动 至多一次 。每个棋子的移动中，首先选择移动的 方向 ，然后选择 移动的步数 ，同时你要确保移动过程中棋子不能移到棋盘以外的地方。棋子需按照以下规则移动：

- 车可以 __水平或者竖直__ 从 `(r, c)` 沿着方向 `(r+1, c)`， `(r-1, c)`， `(r, c+1)` 或者 `(r, c-1)` 移动。
- 后可以 __水平竖直或者斜对角__ 从 `(r, c)` 沿着方向 `(r+1, c)`， `(r-1, c)`， `(r, c+1)`， `(r, c-1)`， `(r+1, c+1)`， `(r+1, c-1)`， `(r-1, c+1)`， `(r-1, c-1)` 移动。
- 象可以 __斜对角__ 从 `(r, c)` 沿着方向 `(r+1, c+1)`， `(r+1, c-1)`， `(r-1, c+1)`， `(r-1, c-1)` 移动。

__移动组合__ 包含所有棋子的 __移动__ 。每一秒，每个棋子都沿着它们选择的方向往前移动 __一步__ ，直到它们到达目标位置。所有棋子从时刻 `0` 开始移动。如果在某个时刻，两个或者更多棋子占据了同一个格子，那么这个移动组合 __不有效__ 。

请你返回 有效 移动组合的数目。

注意：

- 初始时，__不会有两个棋子__ 在 __同一个位置 。__
- 有可能在一个移动组合中，有棋子不移动。
- 如果两个棋子 __直接相邻__ 且两个棋子下一秒要互相占据对方的位置，可以将它们在同一秒内 __交换位置__ 。

__示例:__

示例 1:

![2056-4](https://assets.leetcode.com/uploads/2021/09/23/a1.png)

```text
输入：pieces = ["rook"], positions = [[1,1]]
输出：15
解释：上图展示了棋子所有可能的移动。
```

示例 2：

![2056-5](https://assets.leetcode.com/uploads/2021/09/23/a2.png)

```text
输入：pieces = ["queen"], positions = [[1,1]]
输出：22
解释：上图展示了棋子所有可能的移动。
```

示例 3:

![2056-6](https://assets.leetcode.com/uploads/2021/09/23/a3.png)

```text
输入：pieces = ["bishop"], positions = [[4,3]]
输出：12
解释：上图展示了棋子所有可能的移动。
```

示例 4:

![2056-7](https://assets.leetcode.com/uploads/2021/09/23/a4.png)

```text
输入：pieces = ["rook","rook"], positions = [[1,1],[8,8]]
输出：223
解释：每个车有 15 种移动，所以总共有 15 * 15 = 225 种移动组合。
但是，有两个是不有效的移动组合：
```

- 将两个车都移动到 (8, 1) ，会导致它们在同一个格子相遇。
- 将两个车都移动到 (1, 8) ，会导致它们在同一个格子相遇。

```text
所以，总共有 225 - 2 = 223 种有效移动组合。
注意，有两种有效的移动组合，分别是一个车在 (1, 8) ，另一个车在 (8, 1) 。
即使棋盘状态是相同的，这两个移动组合被视为不同的，因为每个棋子移动操作是不相同的。
```

示例 5：

![2056-8](https://assets.leetcode.com/uploads/2021/09/23/a5.png)

```text
输入：pieces = ["queen","bishop"], positions = [[5,7],[3,4]]
输出：281
解释：总共有 12 * 24 = 288 种移动组合。
但是，有一些不有效的移动组合：
```

- 如果后停在 (6, 7) ，它会阻挡象到达 (6, 7) 或者 (7, 8) 。
- 如果后停在 (5, 6) ，它会阻挡象到达 (5, 6) ，(6, 7) 或者 (7, 8) 。
- 如果象停在 (5, 2) ，它会阻挡后到达 (5, 2) 或者 (5, 1) 。

在 288 个移动组合当中，281 个是有效的。

__提示：__

- `n == pieces.length`
- `n == positions.length`
- `1 <= n <= 4`
- `pieces` 只包含字符串 `"rook"` ， `"queen"` 和 `"bishop"` 。
- 棋盘上总共最多只有一个后。
- `1 <= xi, yi <= 8`
- 每一个 `positions[i]` 互不相同。

__思路:__

```text
DFS
记录皇后和车以及主教的移动方向
皇后可以向 8 个方向移动, 车可以向 4 个方向移动, 主教可以向 4 个方向移动
分别为 [0, 1], [1, 0], [0, -1], [-1, 0], [1, 1], [1, -1], [-1, 1], [-1, -1]
先将棋子直接移动到目标位置, 然后递归遍历所有的移动组合
如果两个棋子在同一个位置, 那么这个移动组合是无效的
或者棋子移动到了棋盘外, 那么这个移动组合也是无效的
否则，这个移动组合是有效的
时间复杂度为 O(N), 空间复杂度为 O(N), 棋盘为 8 X 8 固定值, 其他的复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countCombinations(vector<string>& pieces, vector<vector<int>>& positions) 
    {
        vector<pair<int, int>> pos; 
        for (auto&& e : positions) pos.emplace_back(e[0], e[1]);
        int n = pieces.size(), result = 0;
        auto check = [&](auto&& state) 
        {
            int m = state.size();
            if (m <= 1) return true;
            auto cur = pos;
            for (int j = 0; ; j++) 
            {
                bool flag = false;
                for (int i = 0; i < m; i++) 
                {
                    auto&& [x, y] = cur[i];
                    auto [dx, dy, step] = state[i];
                    if (j < step) 
                    {
                        x += dx; 
                        y += dy;
                        flag = true;
                    }
                }
                for (int i = 0; i < m - 1; i++) for (int j = i + 1; j < m; j++) if (cur[i] == cur[j]) return false;
                if (!flag) return true;
            }
        };
        vector<tuple<int, int, int>> cur;
        auto dfs = [&](auto&& dfs, auto idx) 
        {
            if (not check(cur)) return;
            if (idx == n) return (void)++result;
            auto&& dir = dirs[pieces[idx]];
            auto [x, y] = pos[idx];
            for (int i = 1; i < 8; i++) for (auto [dx, dy] : dir) 
            {
                auto nx = x + dx * i, ny = y + dy * i;
                if (1 <= nx and nx <= 8 and 1 <= ny and ny <= 8) 
                {
                    cur.emplace_back(dx, dy, i);
                    dfs(dfs, idx + 1);
                    cur.pop_back();
                }
            }
            cur.emplace_back();
            dfs(dfs, idx + 1);
            cur.pop_back();
        };
        dfs(dfs, 0);
        return result;
    }
private:
    unordered_map<string, vector<pair<int, int>>> dirs = 
    {
        {"queen", {{1, 0}, {0, 1}, {0, -1}, {-1, 0}, {-1, -1}, {1, 1}, {1, -1}, {-1, 1}}},
        {"rook", {{1, 0}, {0, 1}, {0, -1}, {-1, 0}}},
        {"bishop", {{-1, -1}, {1, 1}, {1, -1}, {-1, 1}}}
    };
};
```

__Java__:

```Java
class Solution {
    int[][] dirs = new int[][] {{0, 1}, {1, 0}, {0, -1}, {-1, 0}, {1, 1}, {1, -1}, {-1, -1}, {-1, 1}};
    int[][][] board;

    public int countCombinations(String[] pieces, int[][] positions) {
        board = new int[positions.length][8][8];
        return count_recursive(pieces, positions, 0);
    }

    public int count_recursive(String[] pieces, int[][] positions, int i) {
        if (i >= pieces.length) return 1;
        int x = positions[i][0] - 1, y = positions[i][1] - 1, result = 0;
        for (int d = 0; d < 8; d++){
            if ((d < 4 && pieces[i].equals("bishop")) || (d >= 4 && pieces[i].equals("rook"))) continue;
            boolean blocked = false;           
            for (int step = result == 0 ? 1 : 2; !blocked; step++) {
                int nx = x + (step - 1) * dirs[d][0], ny = y + (step - 1) * dirs[d][1];
                if (nx < 0 || nx >= 8 || ny < 0 || ny >= 8) break;
                boolean flag = true;
                for (int j = 0; j < i; j++) {
                    flag = flag && (board[j][nx][ny] >= 0) && (board[j][nx][ny] < step);
                    blocked = blocked || ((board[j][nx][ny] < 0) && (-board[j][nx][ny] <= step)) || (board[j][nx][ny] == step);
                }
                if (flag) {
                    board[i][nx][ny] = -step;
                    result += count_recursive(pieces, positions, i + 1);
                }
                board[i][nx][ny] = step;
            }
            board[i] = new int[8][8];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countCombinations(self, pieces: List[str], positions: List[List[int]]) -> int:
        dirs = {"rook": ((0, 1), (0, -1), (1, 0), (-1, 0)), "bishop":  ((1, 1), (1, -1), (-1, 1), (-1, -1)), "queen":   ((0, 1), (0, -1), (1, 0), (-1, 0), (1, 1), (1, -1), (-1, 1), (-1, -1))}

        def get_moves(p: str, x: int, y: int):
            yield x, y, 0, 0, 0
            for t, (dx, dy) in product(range(1, 8), dirs[p]):
                if 1 <= x + dx * t <= 8 and 1 <= y + dy * t <= 8:
                    yield x, y, dx, dy, t

        def check(status: tuple[int, int, int, int, int]) -> bool:
            for t in range(8):
                visited = set()
                for x, y, dx, dy, pt in status:
                    if (px := x + dx * (dt := min(t, pt)), py := y + dy * dt) in visited: 
                        return False
                    visited.add((px, py))
            return True
        return sum(check(status) for status in product(*[get_moves(p, x, y) for p, (x, y) in zip(pieces, positions)]))
```
