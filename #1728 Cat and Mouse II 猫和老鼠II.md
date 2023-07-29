# 1728 Cat and Mouse II 猫和老鼠II

__Description:__

A game is played by a cat and a mouse named Cat and Mouse.

The environment is represented by a `grid` of size `rows x cols`, where each element is a wall, floor, player (Cat, Mouse), or food.

- Players are represented by the characters `'C'`(Cat) `,'M'`(Mouse).
- Floors are represented by the character `'.'` and can be walked on.
- Walls are represented by the character `'#'` and cannot be walked on.
- Food is represented by the character `'F'` and can be walked on.
- There is only one of each character `'C'`, `'M'`, and `'F'` in `grid`.

Mouse and Cat play according to the following rules:

- Mouse __moves first__, then they take turns to move.
- During each turn, Cat and Mouse can jump in one of the four directions (left, right, up, down). They cannot jump over the wall nor outside of the `grid`.
- `catJump, mouseJump` are the maximum lengths Cat and Mouse can jump at a time, respectively. Cat and Mouse can jump less than the maximum length.
- Staying in the same position is allowed.
- Mouse can jump over Cat.

The game can end in 4 ways:

- If Cat occupies the same position as Mouse, Cat wins.
- If Cat reaches the food first, Cat wins.
- If Mouse reaches the food first, Mouse wins.
- If Mouse cannot get to the food within 1000 turns, Cat wins.

Given a `rows x cols` matrix `grid` and two integers `catJump` and `mouseJump`, return `true` _if Mouse can win the game if both Cat and Mouse play optimally, otherwise return_ `false`.

__Example:__

Example 1:

![1728-1](https://assets.leetcode.com/uploads/2020/09/12/sample_111_1955.png)

```text
Input: grid = ["####F","#C...","M...."], catJump = 1, mouseJump = 2
Output: true
Explanation: Cat cannot catch Mouse on its turn nor can it get the food before Mouse.
```

Example 2:

![1728-2](https://assets.leetcode.com/uploads/2020/09/12/sample_2_1955.png)

```text
Input: grid = ["M.C...F"], catJump = 1, mouseJump = 4
Output: true
```

Example 3:

```text
Input: grid = ["M.C...F"], catJump = 1, mouseJump = 3
Output: false
```

__Constraints:__

- `rows == grid.length`
- `cols = grid[i].length`
- `1 <= rows, cols <= 8`
- `grid[i][j]` consist only of characters `'C'`, `'M'`, `'F'`, `'.'`, and `'#'`.
- There is only one of each character `'C'`, `'M'`, and `'F'` in `grid`.
- `1 <= catJump, mouseJump <= 8`

__题目描述:__

一只猫和一只老鼠在玩一个叫做猫和老鼠的游戏。

它们所处的环境设定是一个 `rows x cols` 的方格 `grid` ，其中每个格子可能是一堵墙、一块地板、一位玩家（猫或者老鼠）或者食物。

- 玩家由字符 `'C'` （代表猫）和 `'M'` （代表老鼠）表示。
- 地板由字符 `'.'` 表示，玩家可以通过这个格子。
- 墙用字符 `'#'` 表示，玩家不能通过这个格子。
- 食物用字符 `'F'` 表示，玩家可以通过这个格子。
- 字符 `'C'` ， `'M'` 和 `'F'` 在 `grid` 中都只会出现一次。

猫和老鼠按照如下规则移动：

- 老鼠 __先移动__ ，然后两名玩家轮流移动。
- 每一次操作时，猫和老鼠可以跳到上下左右四个方向之一的格子，他们不能跳过墙也不能跳出 `grid` 。
- `catJump` 和 `mouseJump` 是猫和老鼠分别跳一次能到达的最远距离，它们也可以跳小于最大距离的长度。
- 它们可以停留在原地。
- 老鼠可以跳跃过猫的位置。

游戏有 4 种方式会结束：

- 如果猫跟老鼠处在相同的位置，那么猫获胜。
- 如果猫先到达食物，那么猫获胜。
- 如果老鼠先到达食物，那么老鼠获胜。
- 如果老鼠不能在 1000 次操作以内到达食物，那么猫获胜。

给你 `rows x cols` 的矩阵 `grid` 和两个整数 `catJump` 和 `mouseJump` ，双方都采取最优策略，如果老鼠获胜，那么请你返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

![1728-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/17/sample_111_1955.png)

```text
输入：grid = ["####F","#C...","M...."], catJump = 1, mouseJump = 2
输出：true
解释：猫无法抓到老鼠，也没法比老鼠先到达食物。
```

示例 2：

![1728-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/17/sample_2_1955.png)

```text
输入：grid = ["M.C...F"], catJump = 1, mouseJump = 4
输出：true
```

示例 3：

```text
输入：grid = ["M.C...F"], catJump = 1, mouseJump = 3
输出：false
```

示例 4：

```text
输入：grid = ["C...#","...#F","....#","M...."], catJump = 2, mouseJump = 5
输出：false
```

示例 5：

```text
输入：grid = [".M...","..#..","#..#.","C#.#.","...#F"], catJump = 3, mouseJump = 1
输出：true
```

__提示：__

- `rows == grid.length`
- `cols = grid[i].length`
- `1 <= rows, cols <= 8`
- `grid[i][j]` 只包含字符 `'C'` ， `'M'` ， `'F'` ， `'.'` 和 `'#'` 。
- `grid` 中只包含一个 `'C'` ， `'M'` 和 `'F'` 。
- `1 <= catJump, mouseJump <= 8`

__思路:__

```text
拓扑排序
对老鼠来说, 其必胜态和必败态如下:
必胜态: 无论对方如何移动, 都可以获胜的状态. 在本局游戏中, 必胜态仅有一种: 老鼠到达食物
必败态: 无论如何移动, 都会输掉的状态. 在本局游戏中, 必败态有两种: 猫到达食物, 老鼠无法在 1000 次操作以内到达食物, 猫和老鼠处于同一位置
用 dp[catx][caty][mousex][mousey][turn] 表示老鼠在 (mousex, mousey) 位置, 猫在 (catx, caty) 位置, 当前是谁的回合, 该状态是必胜态还是必败态, 必胜态为 1, 必败态为 -1, 未知为 0, turn 为 0 表示老鼠回合, 为 1 表示猫回合
对于一个必败态, 其相邻的状态必然是必胜态, 从一个必败态开始进行 BFS, 将其相邻的状态标记为必胜态
对于一个必胜态, 那么可以将其相邻的状态的度数减 1, 如果一个状态度数为 0 且这个状态还没有被计算过, 说明其相邻的状态都是必胜态, 其本身是必败态, 则将其加入队列
找到老鼠, 猫和食物的位置
预先计算所有的态的度数
预先计算已知的必胜态和必败态
从队列中取出状态依次将相邻的状态进行修改直到队列为空
最后判断初始状态是否为必胜态, 只要不为必胜态就是失败返回 false
时间复杂度为 O(M ^ 2 * N ^ 2 * (M + N)), 空间复杂度为 O(M ^ 2 * N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    bool canMouseWin(vector<string>& grid, int catJump, int mouseJump) 
    {
        int m = grid.size(), n = grid.front().size();
        auto find_position = [&](char c) -> pair<int, int> 
        {
            for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] == c) return {i, j};
            return {-1, -1};
        };
        auto get_neighbors = [&](int x, int y, int bound) -> vector<pair<int, int>> 
        {
            vector<pair<int, int>> result = {{x, y}};
            for (const auto [dx, dy] : dirs) 
            {
                int xx = x, yy = y;
                for (int i = 1; i <= bound; i++) 
                {
                    xx += dx;
                    yy += dy;
                    if (xx < 0 or xx >= m or yy < 0 or yy >= n or grid[xx][yy] == '#') break;
                    result.emplace_back(xx, yy);
                }
            }
            return result;
        };
        auto [cx, cy] = find_position('C');
        auto [mx, my] = find_position('M'); 
        auto [fx, fy] = find_position('F');
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (grid[i][j] != '#') 
                {
                    for (int k = 0; k < m; k++) 
                    {
                        for (int l = 0; l < n; l++) 
                        {
                            if (grid[k][l] != '#') 
                            {
                                degree[i][j][k][l][0] = get_neighbors(k, l, mouseJump).size();
                                degree[i][j][k][l][1] = get_neighbors(i, j, catJump).size();
                            }
                        }
                    }
                }
            }
        }
        memset(dp, 0, sizeof dp);
        queue<tuple<int, int, int, int, int>> q;
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (grid[i][j] != '#' and grid[i][j] != 'F') 
                {
                    dp[i][j][i][j][0] = -1;
                    dp[i][j][i][j][1] = 1;
                    q.emplace(i, j, i, j, 0);
                    q.emplace(i, j, i, j, 1);
                    dp[fx][fy][i][j][0] = -1;
                    dp[i][j][fx][fy][1] = -1;
                    q.emplace(fx, fy, i, j, 0);
                    q.emplace(i, j, fx, fy, 1);
                }
            }
        }
        while (!q.empty()) 
        {
            auto [catx, caty, mousex, mousey, turn] = q.front();
            q.pop();
            if (turn) 
            {
                for (const auto [x, y]: get_neighbors(mousex, mousey, mouseJump)) 
                {
                    --degree[catx][caty][x][y][turn ^ 1];
                    if (!dp[catx][caty][x][y][turn ^ 1]) 
                    {
                        if (dp[catx][caty][mousex][mousey][turn] == -1) 
                        {
                            dp[catx][caty][x][y][turn ^ 1] = 1;
                            q.emplace(catx, caty, x, y, turn ^ 1);
                        }
                        else if (!degree[catx][caty][x][y][turn ^ 1]) 
                        {
                            dp[catx][caty][x][y][turn ^ 1] = -1;
                            q.emplace(catx, caty, x, y, turn ^ 1);
                        }
                    }
                }
            }
            else 
            {
                for (const auto [x, y]: get_neighbors(catx, caty, catJump)) 
                {
                    --degree[x][y][mousex][mousey][turn ^ 1];
                    if (!dp[x][y][mousex][mousey][turn ^ 1]) 
                    { 
                        if (dp[catx][caty][mousex][mousey][turn] == -1) 
                        {
                            dp[x][y][mousex][mousey][turn ^ 1] = 1;
                            q.emplace(x, y, mousex, mousey, turn ^ 1);
                        }
                        else if (!degree[x][y][mousex][mousey][turn ^ 1]) 
                        {
                            dp[x][y][mousex][mousey][turn ^ 1] = -1;
                            q.emplace(x, y, mousex, mousey, turn ^ 1);
                        }
                    }
                }
            }
        }
        return dp[cx][cy][mx][my][0] == 1;
    }
private:
    int dp[8][8][8][8][2];
    int degree[8][8][8][8][2];
    static constexpr int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
};
```

__Java__:

```Java
class Solution {
    private int[][][][][] dp = new int[8][8][8][8][2];
    private int[][][][][] degree = new int[8][8][8][8][2];
    private int[][] dirs = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    private int m, n;
    private String[] grid;
    
    public boolean canMouseWin(String[] grid, int catJump, int mouseJump) {
        m = grid.length;
        n = grid[0].length();
        this.grid = grid;
        int cx = 0, cy = 0, mx = 0, my = 0, fx = 0, fy = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i].charAt(j) == 'C') {
                    cx = i;
                    cy = j;
                }
                if (grid[i].charAt(j) == 'M') {
                    mx = i;
                    my = j;
                }
                if (grid[i].charAt(j) == 'F') {
                    fx = i;
                    fy = j;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i].charAt(j) != '#') {
                    for (int k = 0; k < m; k++) {
                        for (int l = 0; l < n; l++) {
                            if (grid[k].charAt(l) != '#') {
                                degree[i][j][k][l][0] = getNeighbors(k, l, mouseJump).size();
                                degree[i][j][k][l][1] = getNeighbors(i, j, catJump).size();
                            }
                        }
                    }
                }
            }
        }
        Queue<int[]> q = new LinkedList<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i].charAt(j) != '#' && grid[i].charAt(j) != 'F') {
                    dp[i][j][i][j][0] = -1;
                    dp[i][j][i][j][1] = 1;
                    q.offer(new int[]{i, j, i, j, 0});
                    q.offer(new int[]{i, j, i, j, 1});
                    dp[fx][fy][i][j][0] = -1;
                    dp[i][j][fx][fy][1] = -1;
                    q.offer(new int[]{fx, fy, i, j, 0});
                    q.offer(new int[]{i, j, fx, fy, 1});
                }
            }
        }
        while (!q.isEmpty()) 
        {
            int[] cur = q.poll();
            int catx = cur[0], caty = cur[1], mousex = cur[2], mousey = cur[3], turn = cur[4];
            if (turn == 1) {
                for (int[] steps : getNeighbors(mousex, mousey, mouseJump)) {
                    int x = steps[0], y = steps[1];
                    --degree[catx][caty][x][y][turn ^ 1];
                    if (dp[catx][caty][x][y][turn ^ 1] == 0) {
                        if (dp[catx][caty][mousex][mousey][turn] == -1) {
                            dp[catx][caty][x][y][turn ^ 1] = 1;
                            q.offer(new int[]{catx, caty, x, y, turn ^ 1});
                        }
                        else if (degree[catx][caty][x][y][turn ^ 1] == 0) {
                            dp[catx][caty][x][y][turn ^ 1] = -1;
                            q.offer(new int[]{catx, caty, x, y, turn ^ 1});
                        }
                    }
                }
            }
            else 
            {
                for (int[] steps : getNeighbors(catx, caty, catJump)) {
                    int x = steps[0], y = steps[1];
                    --degree[x][y][mousex][mousey][turn ^ 1];
                    if (dp[x][y][mousex][mousey][turn ^ 1] == 0) { 
                        if (dp[catx][caty][mousex][mousey][turn] == -1) {
                            dp[x][y][mousex][mousey][turn ^ 1] = 1;
                            q.offer(new int[]{x, y, mousex, mousey, turn ^ 1});
                        }
                        else if (degree[x][y][mousex][mousey][turn ^ 1] == 0) {
                            dp[x][y][mousex][mousey][turn ^ 1] = -1;
                            q.offer(new int[]{x, y, mousex, mousey, turn ^ 1});
                        }
                    }
                }
            }
        }
        return dp[cx][cy][mx][my][0] == 1;
    }
    
    private List<int[]> getNeighbors(int x, int y, int bound) {
        List<int[]> result = new ArrayList<>();
        result.add(new int[]{x, y});
        for (int d = 0; d < 4; d++) {
            int xx = x, yy = y;
            for (int i = 0; i < bound; i++) {
                xx += dirs[d][0];
                yy += dirs[d][1];
                if (xx < 0 || xx >= m || yy < 0 || yy >= n || grid[xx].charAt(yy) == '#') break;
                result.add(new int[]{xx, yy});
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def canMouseWin(self, grid: List[str], catJump: int, mouseJump: int) -> bool:
        m, n, dp, degree, d, q = len(grid), len(grid[0]), defaultdict(int), defaultdict(int), [[-1, 0], [1, 0], [0, -1], [0, 1]], deque()
        
        def find_position(c: str) -> Tuple[int, int]:
            for i in range(m):
                for j in range(n):
                    if grid[i][j] == c:
                        return i, j
                    
        def get_neighbors(x: int, y: int, bound: int) -> List[List[int]]:
            result = [[x, y]]
            for dx, dy in d:
                xx, yy = x, y
                for _ in range(bound):
                    xx += dx
                    yy += dy
                    if xx < 0 or xx >= m or yy < 0 or yy >= n or grid[xx][yy] == '#':
                        break
                    result.append([xx, yy])
            return result
        (cx, cy), (mx, my), (fx, fy) = find_position('C'), find_position('M'), find_position('F')
        for i in range(m):
            for j in range(n):
                if grid[i][j] != '#':
                    for k in range(m):
                        for l in range(n):
                            if grid[i][j] != '#':
                                degree[(i, j, k, l, 0)], degree[(i, j, k, l, 1)] = len(get_neighbors(k, l, mouseJump)), len(get_neighbors(i, j, catJump))
        for i in range(m):
            for j in range(n):
                if grid[i][j] != '#' and grid[i][j] != 'F':
                    dp[(i, j, i, j, 0)], dp[(i, j, i, j, 1)], dp[(fx, fy, i, j, 0)], dp[(i, j, fx, fy, 1)] = -1, 1, -1, -1
                    q.append((i, j, i, j, 0))
                    q.append((i, j, i, j, 1))
                    q.append((fx, fy, i, j, 0))
                    q.append((i, j, fx, fy, 1))
        while q:
            catx, caty, mousex, mousey, turn = q.pop();
            if turn:
                for x, y in get_neighbors(mousex, mousey, mouseJump):
                    degree[(catx, caty, x, y, turn ^ 1)] -= 1
                    if not dp[(catx, caty, x, y, turn ^ 1)]:
                        if dp[(catx, caty, mousex, mousey, turn)] == -1:
                            dp[(catx, caty, x, y, turn ^ 1)] = 1
                            q.append((catx, caty, x, y, turn ^ 1))
                        elif not degree[(catx, caty, x, y, turn ^ 1)]:
                            dp[(catx, caty, x, y, turn ^ 1)] = -1
                            q.append((catx, caty, x, y, turn ^ 1))
            else:
                for x, y in get_neighbors(catx, caty, catJump):
                    degree[(x, y, mousex, mousey, turn ^ 1)] -= 1
                    if not dp[(x, y, mousex, mousey, turn ^ 1)]:
                        if dp[(catx, caty, mousex, mousey, turn)] == -1:
                            dp[(x, y, mousex, mousey, turn ^ 1)] = 1
                            q.append((x, y, mousex, mousey, turn ^ 1))
                        elif not degree[(x, y, mousex, mousey, turn ^ 1)]:
                            dp[(x, y, mousex, mousey, turn ^ 1)] = -1
                            q.append((x, y, mousex, mousey, turn ^ 1))
        return dp[(cx, cy, mx, my, 0)] == 1
```
