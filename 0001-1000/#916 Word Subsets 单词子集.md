# 913 Cat and Mouse 猫和老鼠

__Description__:
A game on an undirected graph is played by two players, Mouse and Cat, who alternate turns.

The graph is given as follows: graph[a] is a list of all nodes b such that ab is an edge of the graph.

The mouse starts at node 1 and goes first, the cat starts at node 2 and goes second, and there is a hole at node 0.

During each player's turn, they must travel along one edge of the graph that meets where they are.  For example, if the Mouse is at node 1, it must travel to any node in graph[1].

Additionally, it is not allowed for the Cat to travel to the Hole (node 0.)

Then, the game can end in three ways:

If ever the Cat occupies the same node as the Mouse, the Cat wins.
If ever the Mouse reaches the Hole, the Mouse wins.
If ever a position is repeated (i.e., the players are in the same position as a previous turn, and it is the same player's turn to move), the game is a draw.
Given a graph, and assuming both players play optimally, return

1 if the mouse wins the game,
2 if the cat wins the game, or
0 if the game is a draw.

__Example:__

Example 1:

![cat1](https://upload-images.jianshu.io/upload_images/16639143-314895fd368b09d5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Input: graph = [[2,5],[3],[0,4,5],[1,4,5],[2,3],[0,2,3]]
Output: 0

Example 2:

![cat2](https://upload-images.jianshu.io/upload_images/16639143-8de2aae135de783f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Input: graph = [[1,3],[0],[3],[0,2]]
Output: 1

__Constraints:__

3 <= graph.length <= 50
1 <= graph[i].length < graph.length
0 <= graph[i][j] < graph.length
graph[i][j] != i
graph[i] is unique.
The mouse and the cat can always move.

__题目描述__:
两个玩家分别扮演猫（Cat）和老鼠（Mouse）在无向图上进行游戏，他们轮流行动。

该图按下述规则给出：graph[a] 是所有结点 b 的列表，使得 ab 是图的一条边。

老鼠从结点 1 开始并率先出发，猫从结点 2 开始且随后出发，在结点 0 处有一个洞。

在每个玩家的回合中，他们必须沿着与他们所在位置相吻合的图的一条边移动。例如，如果老鼠位于结点 1，那么它只能移动到 graph[1] 中的（任何）结点去。

此外，猫无法移动到洞（结点 0）里。

然后，游戏在出现以下三种情形之一时结束：

如果猫和老鼠占据相同的结点，猫获胜。
如果老鼠躲入洞里，老鼠获胜。
如果某一位置重复出现（即，玩家们的位置和移动顺序都与上一个回合相同），游戏平局。
给定 graph，并假设两个玩家都以最佳状态参与游戏，如果老鼠获胜，则返回 1；如果猫获胜，则返回 2；如果平局，则返回 0。

__示例 :__

输入：[[2,5],[3],[0,4,5],[1,4,5],[2,3],[0,2,3]]
输出：0
解释：

```text
4---3---1
|   |
2---5
 \ /
  0
```

__提示:__

3 <= graph.length <= 200
保证 graph[1] 非空。
保证 graph[2] 包含非零元素。

__思路__:

动态规划
记录每一步的状态, dp[t][x][y] 表示走了 t 步, 老鼠在 x 位置, 猫在 y 位置, 当前的胜利状态
dp[t][x][y] = 0 表示平局, dp[t][x][y] = 1 表示老鼠获胜, dp[t][x][y] = 2 表示猫获胜
dp 最多只需要 2 \* n 步, 因为猫和老鼠每个分别可以走 n 步, 2 \* n 步之后必然发生重复, 超过范围必然是平局, 返回 0
如果当前 x == y, 即老鼠和猫相遇, 那么是猫获胜
如果当前 x == 0, 即老鼠回洞, 则是由老鼠获胜
如果当前 dp[t][x][y] != -1, 说明已经搜索过这种情况, 不需要搜索, 直接返回 dp[t][x][y]
否则按照 t 的奇偶性进入选择
如果 t 为偶数, 当前行动的是老鼠, 用 BFS 的方式搜索所有老鼠当前位置能去的点 next, 如果 next 为 1 则老鼠必胜, 如果 next == 0 说明下一步还是平局, 否则如果怎么走都是猫获胜就是猫获胜
t 为奇数情况类似
注意猫不能进入洞中
时间复杂度为 O(n ^ 3), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int catMouseGame(vector<vector<int>>& graph) 
    {
        int n = graph.size();
        vector<vector<vector<int>>> dp(n << 1, vector<vector<int>>(n, vector<int>(n, -1)));
        return helper(graph, 0, 1, 2, dp);
    }
private:
    int helper(vector<vector<int>>& graph, int t, int x, int y, vector<vector<vector<int>>>& dp) 
    {
        if (t == dp.size()) return 0;
        if (x == y) return dp[t][x][y] = 2;
        if (!x) return dp[t][x][y] = 1;
        if (dp[t][x][y] != -1) return dp[t][x][y];
        if (!(t & 1)) 
        {
            bool cat = true;
            for (int i = 0; i < graph[x].size(); i++) 
            {
                int next = helper(graph, t + 1, graph[x][i], y, dp);
                if (next == 1) return dp[t][x][y] = 1;
                else if (next != 2) cat = false;
            }
            if (cat) return dp[t][x][y] = 2;
        } 
        else 
        {
            bool mouse = true;
            for (int i = 0; i < graph[y].size(); i++) 
            {
                if (graph[y][i] == 0) continue;
                int next = helper(graph, t + 1, x, graph[y][i], dp);
                if (next == 2) return dp[t][x][y] = 2;
                else if (next != 1) mouse = false;
            }
            if (mouse) return dp[t][x][y] = 1;
        }
        return dp[t][x][y] = 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int catMouseGame(int[][] graph) {
        int n = graph.length, dp[][][] = new int[n << 1][n][n];
        for (int i = 0; i < (n << 1); i++) for (int j = 0; j < n; j++) Arrays.fill(dp[i][j], -1);
        return helper(graph, dp, 0, 1, 2);
    }
    
    private int helper(int[][] graph, int[][][] dp, int t, int x, int y) {
        if (t == dp.length) return 0;
        if (x == y) return dp[t][x][y] = 2;
        if (x == 0) return dp[t][x][y] = 1;
        if (dp[t][x][y] != -1) return dp[t][x][y];
        if ((t & 1) != 0) {
            boolean mouse = true;
            for (int i = 0; i < graph[y].length; i++) {
                if (graph[y][i] == 0) continue;
                int next = helper(graph, dp, t + 1, x, graph[y][i]);
                if (next == 2) return dp[t][x][y] = 2;
                else if (next != 1) mouse = false;
            }
            if (mouse) return dp[t][x][y] = 1;
        } else {
            boolean cat = true;
            for (int i = 0; i < graph[x].length; i++) {
                int next = helper(graph, dp, t + 1, graph[x][i], y);
                if (next == 1) return dp[t][x][y] = 1;
                else if (next != 2) cat = false;
            }
            if (cat) return dp[t][x][y] = 2;
        }
        return dp[t][x][y] = 0;
    }
}
```

__Python__:

```Python
class Solution:
    def catMouseGame(self, graph: List[List[int]]) -> int:
        @lru_cache(None)
        def search(t: int, x: int, y: int) -> int:
            return 0 if t == (len(graph) << 1) else 2 if x == y else 1 if not x else 2 if (t & 1 and any(search(t + 1, x, i) == 2 for i in graph[y] if i)) else 0 if (t & 1 and any(search(t + 1, x, i) == 0 for i in graph[y] if i)) else 1 if t & 1 else 1 if any(search(t + 1, i, y) == 1 for i in graph[x]) else 0 if any(search(t + 1, i, y) == 0 for i in graph[x]) else 2
        return search(0, 1, 2)
```
