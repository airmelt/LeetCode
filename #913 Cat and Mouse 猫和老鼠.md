# 913 Cat and Mouse 猫和老鼠

__Description:__

A game on an undirected graph is played by two players, Mouse and Cat, who alternate turns.

The graph is given as follows: `graph[a]` is a list of all nodes `b` such that `ab` is an edge of the graph.

The mouse starts at node `1` and goes first, the cat starts at node `2` and goes second, and there is a hole at node `0`.

During each player's turn, they __must__ travel along one edge of the graph that meets where they are. For example, if the Mouse is at node 1, it __must__ travel to any node in `graph[1]`.

Additionally, it is not allowed for the Cat to travel to the Hole (node `0`).

Then, the game can end in three ways:

- If ever the Cat occupies the same node as the Mouse, the Cat wins.
- If ever the Mouse reaches the Hole, the Mouse wins.
- If ever a position is repeated (i.e., the players are in the same position as a previous turn, and it is the same player's turn to move), the game is a draw.

Given a `graph`, and assuming both players play optimally, return

- `1` if the mouse wins the game,
- `2` if the cat wins the game, or
- `0` if the game is a draw.

__Example:__

Example 1:

![913-1](https://assets.leetcode.com/uploads/2020/11/17/cat1.jpg)

```text
Input: graph = [[2,5],[3],[0,4,5],[1,4,5],[2,3],[0,2,3]]
Output: 0
```

Example 2:

![913-2](https://assets.leetcode.com/uploads/2020/11/17/cat2.jpg)

```text
Input: graph = [[1,3],[0],[3],[0,2]]
Output: 1
```

__Constraints:__

- `3 <= graph.length <= 50`
- `1 <= graph[i].length < graph.length`
- `0 <= graph[i][j] < graph.length`
- `graph[i][j] != i`
- `graph[i]` is unique.
- The mouse and the cat can always move.

__题目描述:__

两位玩家分别扮演猫和老鼠，在一张 无向 图上进行游戏，两人轮流行动。

图的形式是: `graph[a]` 是一个列表，由满足 `ab` 是图中的一条边的所有节点 `b` 组成。

老鼠从节点 `1` 开始，第一个出发；猫从节点 `2` 开始，第二个出发。在节点 `0` 处有一个洞。

在每个玩家的行动中，他们 __必须__ 沿着图中与所在当前位置连通的一条边移动。例如，如果老鼠在节点 `1` ，那么它必须移动到 `graph[1]` 中的任一节点。

此外，猫无法移动到洞中（节点 `0`）。

然后，游戏在出现以下三种情形之一时结束：

- 如果猫和老鼠出现在同一个节点，猫获胜。
- 如果老鼠到达洞中，老鼠获胜。
- 如果某一位置重复出现（即，玩家的位置和移动顺序都与上一次行动相同），游戏平局。

给你一张图 `graph` ，并假设两位玩家都都以最佳状态参与游戏:

- 如果老鼠获胜，则返回 `1`；
- 如果猫获胜，则返回 `2`；
- 如果平局，则返回 `0` 。

__示例:__

示例 1：

![913-3](https://assets.leetcode.com/uploads/2020/11/17/cat1.jpg)

```text
输入：graph = [[2,5],[3],[0,4,5],[1,4,5],[2,3],[0,2,3]]
输出：0
```

示例 2：

![913-4](https://assets.leetcode.com/uploads/2020/11/17/cat2.jpg)

```text
输入：graph = [[1,3],[0],[3],[0,2]]
输出：1
```

__提示：__

- `3 <= graph.length <= 50`
- `1 <= graph[i].length < graph.length`
- `0 <= graph[i][j] < graph.length`
- `graph[i][j] != i`
- `graph[i]` 互不相同
- 猫和老鼠在游戏中总是可以移动

__思路:__

```text
动态规划 ➕ 拓扑排序
设 dp[mouse][cat][turn] 表示老鼠在 mouse 节点，猫在 cat 节点，当前轮到的是 turn 的玩家时的游戏结果
turn = 0 表示老鼠，turn = 1 表示猫
dp[mouse][cat][turn] = 0 表示平局，dp[mouse][cat][turn] = 1 表示老鼠获胜，dp[mouse][cat][turn] = 2 表示猫获胜
初始化 dp[HOLE][i][MOUSE_TURN] = dp[HOLE][i][CAT_TURN] = MOUSE_WIN, dp[i][i][MOUSE_TURN] = dp[i][i][CAT_TURN] = CAT_WIN
并将上面的初始化状态加入队列 q 中
队列中的每一个状态都是两个必胜态之一
给每一个能够到达的状态进行拓扑排序
记录每个节点能够到达的状态数 turns[mouse][cat][turn]
初始化 turns[mouse][cat][MOUSE_TURN] = graph[mouse].size()
初始化 turns[mouse][cat][CAT_TURN] = graph[cat].size()
由于猫不能到达洞，所以对于 graph[HOLE] 中的每一个节点 j，turns[mouse][cat][CAT_TURN] -= 1
遍历 q 中的每一个状态，如果当前状态为 DRAW，且当前状态的玩家可以获胜，则将当前状态加入队列 q 中, 并且此时它的上一个状态也可以获胜
否则，如果当前状态为 DRAW，且当前状态的玩家无法获胜，则将当前状态的 turns 数减一, 如果 turns 数为 0，则当前状态的玩家获胜, 上一次的玩家无论如何也走不到必胜态, 实际上对于他来说就是必败态
最后返回 dp[MOUSE_START][CAT_START][MOUSE_TURN]
时间复杂度为 O(N ^ 3), 空间复杂度为 O(N ^ 2), 一共有 N ^ 2 种状态, 每次还需要 O(N) 的时间进行拓扑排序计算状态
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int catMouseGame(vector<vector<int>>& graph) 
    {
        int n = graph.size();
        vector<vector<vector<int>>> turns(n, vector<vector<int>>(n, vector<int>(2))), dp(n, vector<vector<int>>(n, vector<int>(2)));
        queue<tuple<int, int, int>> q;
        for (int i = 0; i < n; i++)
        {
            for (int j = 1; j < n; j++)
            {
                turns[i][j][MOUSE_TURN] = graph[i].size();
                turns[i][j][CAT_TURN] = graph[j].size();
            }
        }
        for (int i = 0; i < n; i++) for (int j : graph[HOLE]) --turns[i][j][CAT_TURN];
        for (int i = 1; i < n; i++)
        {
            dp[HOLE][i][MOUSE_TURN] = dp[HOLE][i][CAT_TURN] = MOUSE_WIN;
            q.emplace(HOLE, i, MOUSE_TURN);
            q.emplace(HOLE, i, CAT_TURN);
        }
        for (int i = 1; i < n; i++)
        {
            dp[i][i][MOUSE_TURN] = dp[i][i][CAT_TURN] = CAT_WIN;
            q.emplace(i, i, MOUSE_TURN);
            q.emplace(i, i, CAT_TURN);
        }
        while (!q.empty())
        {
            auto [mouse, cat, turn] = q.front();
            q.pop();
            int cur = dp[mouse][cat][turn];
            vector<tuple<int, int, int>> pre_states;
            if (turn == CAT_TURN) for (int pre : graph[mouse]) pre_states.emplace_back(pre, cat, MOUSE_TURN);
            else for (int pre : graph[cat]) if (pre) pre_states.emplace_back(mouse, pre, CAT_TURN);
            for (const auto& [pre_mouse, pre_cat, pre_turn] : pre_states)
            {
                if (dp[pre_mouse][pre_cat][pre_turn] == DRAW)
                {
                    if ((cur == MOUSE_WIN and pre_turn == MOUSE_TURN) or (cur == CAT_WIN and pre_turn == CAT_TURN))
                    {
                        dp[pre_mouse][pre_cat][pre_turn] = cur;
                        q.emplace(pre_mouse, pre_cat, pre_turn);
                    }
                    else
                    {
                        if (!--turns[pre_mouse][pre_cat][pre_turn])
                        {
                            dp[pre_mouse][pre_cat][pre_turn] = pre_turn == MOUSE_TURN ? CAT_WIN : MOUSE_WIN;
                            q.emplace(pre_mouse, pre_cat, pre_turn);
                        }
                    }
                }
            }
        }
        return dp[MOUSE_START][CAT_START][MOUSE_TURN];
    }
private:
    const int MOUSE_WIN = 1, CAT_WIN = 2, DRAW = 0, MOUSE_TURN = 0, CAT_TURN = 1, HOLE = 0, MOUSE_START = 1, CAT_START = 2;
};
```

__Java__:

```Java
class Solution:
    def catMouseGame(self, graph: List[List[int]]) -> int:
        HOLE, MOUSE_START, CAT_START, MOUSE_TURN, CAT_TURN, DRAW, MOUSE_WIN, CAT_WIN, n = 0, 1, 2, 0, 1, 0, 1, 2, len(graph)
        turns, dp, q = [[[0, 0] for _ in range(n)] for _ in range(n)], [[[0, 0] for _ in range(n)] for _ in range(n)], deque()
        for i in range(n):
            for j in range(1, n):
                turns[i][j][MOUSE_TURN] = len(graph[i])
                turns[i][j][CAT_TURN] = len(graph[j])
        for i in range(n):
            for j in graph[HOLE]:
                turns[i][j][CAT_TURN] -= 1
        for i in range(1, n):
            dp[HOLE][i][MOUSE_TURN] = dp[HOLE][i][CAT_TURN] = MOUSE_WIN
            q.append((HOLE, i, MOUSE_TURN))
            q.append((HOLE, i, CAT_TURN))
        for i in range(1, n):
            dp[i][i][MOUSE_TURN] = dp[i][i][CAT_TURN] = CAT_WIN
            q.append((i, i, MOUSE_TURN))
            q.append((i, i, CAT_TURN))
        while q:
            mouse, cat, turn = q.popleft()
            cur, pre_states = dp[mouse][cat][turn], [(mouse, pre, CAT_TURN) for pre in graph[cat]] if turn == MOUSE_TURN else [(pre, cat, MOUSE_TURN) for pre in graph[mouse] if cat]
            for pre_mouse, pre_cat, pre_turn in pre_states:
                if dp[pre_mouse][pre_cat][pre_turn] == DRAW:
                    if (cur == MOUSE_WIN and pre_turn == MOUSE_TURN) or (cur == CAT_WIN and pre_turn == CAT_TURN):
                        dp[pre_mouse][pre_cat][pre_turn] = cur
                        q.append((pre_mouse, pre_cat, pre_turn))
                    else:
                        turns[pre_mouse][pre_cat][pre_turn] -= 1
                        if not turns[pre_mouse][pre_cat][pre_turn]:
                            dp[pre_mouse][pre_cat][pre_turn] = CAT_WIN if pre_turn == MOUSE_TURN else MOUSE_WIN
                            q.append((pre_mouse, pre_cat, pre_turn))
        return dp[MOUSE_START][CAT_START][MOUSE_TURN]
```

__Python__:

```Python
class Solution:
    def catMouseGame(self, graph: List[List[int]]) -> int:
        HOLE, MOUSE_START, CAT_START, MOUSE_TURN, CAT_TURN, DRAW, MOUSE_WIN, CAT_WIN, n = 0, 1, 2, 0, 1, 0, 1, 2, len(graph)
        turns, dp, q = [[[0, 0] for _ in range(n)] for _ in range(n)], [[[0, 0] for _ in range(n)] for _ in range(n)], deque()
        for i in range(n):
            for j in range(1, n):
                turns[i][j][MOUSE_TURN] = len(graph[i])
                turns[i][j][CAT_TURN] = len(graph[j])
        for i in range(n):
            for j in graph[HOLE]:
                turns[i][j][CAT_TURN] -= 1
        for i in range(1, n):
            dp[HOLE][i][MOUSE_TURN] = dp[HOLE][i][CAT_TURN] = MOUSE_WIN
            q.append((HOLE, i, MOUSE_TURN))
            q.append((HOLE, i, CAT_TURN))
        for i in range(1, n):
            dp[i][i][MOUSE_TURN] = dp[i][i][CAT_TURN] = CAT_WIN
            q.append((i, i, MOUSE_TURN))
            q.append((i, i, CAT_TURN))
        while q:
            mouse, cat, turn = q.popleft()
            cur, pre_states = dp[mouse][cat][turn], [(mouse, pre, CAT_TURN) for pre in graph[cat]] if turn == MOUSE_TURN else [(pre, cat, MOUSE_TURN) for pre in graph[mouse] if cat]
            for pre_mouse, pre_cat, pre_turn in pre_states:
                if dp[pre_mouse][pre_cat][pre_turn] == DRAW:
                    if (cur == MOUSE_WIN and pre_turn == MOUSE_TURN) or (cur == CAT_WIN and pre_turn == CAT_TURN):
                        dp[pre_mouse][pre_cat][pre_turn] = cur
                        q.append((pre_mouse, pre_cat, pre_turn))
                    else:
                        turns[pre_mouse][pre_cat][pre_turn] -= 1
                        if not turns[pre_mouse][pre_cat][pre_turn]:
                            dp[pre_mouse][pre_cat][pre_turn] = CAT_WIN if pre_turn == MOUSE_TURN else MOUSE_WIN
                            q.append((pre_mouse, pre_cat, pre_turn))
        return dp[MOUSE_START][CAT_START][MOUSE_TURN]
```
