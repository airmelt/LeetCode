# 1036 Escape a Large Maze 逃离大迷宫

__Description__:
There is a 1 million by 1 million grid on an XY-plane, and the coordinates of each grid square are (x, y).

We start at the source = [sx, sy] square and want to reach the target = [tx, ty] square. There is also an array of blocked squares, where each blocked[i] = [xi, yi] represents a blocked square with coordinates (xi, yi).

Each move, we can walk one square north, east, south, or west if the square is not in the array of blocked squares. We are also not allowed to walk outside of the grid.

Return true if and only if it is possible to reach the target square from the source square through a sequence of valid moves.

__Example:__

Example 1:

Input: blocked = [[0,1],[1,0]], source = [0,0], target = [0,2]
Output: false
Explanation: The target square is inaccessible starting from the source square because we cannot move.
We cannot move north or east because those squares are blocked.
We cannot move south or west because we cannot go outside of the grid.

Example 2:

Input: blocked = [], source = [0,0], target = [999999,999999]
Output: true
Explanation: Because there are no blocked cells, it is possible to reach the target square.

__Constraints:__

0 <= blocked.length <= 200
blocked[i].length == 2
0 <= xi, yi < 10^6
source.length == target.length == 2
0 <= sx, sy, tx, ty < 10^6
source != target
It is guaranteed that source and target are not blocked.

__题目描述__:
在一个 10^6 x 10^6 的网格中，每个网格上方格的坐标为 (x, y) 。

现在从源方格 source = [sx, sy] 开始出发，意图赶往目标方格 target = [tx, ty] 。数组 blocked 是封锁的方格列表，其中每个 blocked[i] = [xi, yi] 表示坐标为 (xi, yi) 的方格是禁止通行的。

每次移动，都可以走到网格中在四个方向上相邻的方格，只要该方格 不 在给出的封锁列表 blocked 上。同时，不允许走出网格。

只有在可以通过一系列的移动从源方格 source 到达目标方格 target 时才返回 true。否则，返回 false。

__示例 :__

示例 1：

输入：blocked = [[0,1],[1,0]], source = [0,0], target = [0,2]
输出：false
解释：
从源方格无法到达目标方格，因为我们无法在网格中移动。
无法向北或者向东移动是因为方格禁止通行。
无法向南或者向西移动是因为不能走出网格。

示例 2：

输入：blocked = [], source = [0,0], target = [999999,999999]
输出：true
解释：
因为没有方格被封锁，所以一定可以到达目标方格。

__提示:__

0 <= blocked.length <= 200
blocked[i].length == 2
0 <= xi, yi < 106
source.length == target.length == 2
0 <= sx, sy, tx, ty < 10^6
source != target
题目数据保证 source 和 target 不在封锁列表内

__思路__:

BFS
由于棋盘过大, 仅使用 BFS 找到两个点之间的通路可能会 TLE
考虑到 blocked 最大只有 200 个点
如果 target 和 source 可到达的点超过 200 个, 就不可能被围住
所以可以在超过 200 时进行剪枝
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isEscapePossible(vector<vector<int>>& blocked, vector<int>& source, vector<int>& target) 
    {
        unordered_set<long> s, t, v;
        for (const auto& block : blocked) s.insert(block.front() * 1001L + block.back());
        return dfs(source.front(), source.back(), s, source, target, t) and dfs(target.front(), target.back(), s, target, source, v);
    }
private:
    bool dfs(int x, int y, unordered_set<long>& s, vector<int>& source, vector<int>& target, unordered_set<long>& visited) 
    {
        if (x == target.front() and y == target.back()) return true;
        long hash = x * 1001L + y;
        if (s.count(hash) or visited.count(hash) or x < 0 or x > 999999 or y < 0 or y > 999999) return false;
        if (abs(source.front() - x) + abs(source.back() - y) > 200) return true;
        visited.insert(hash);
        return dfs(x + 1, y, s, source, target, visited) or dfs(x - 1, y, s, source, target, visited) or dfs(x, y + 1, s, source, target, visited) or dfs(x, y - 1, s, source, target, visited);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isEscapePossible(int[][] blocked, int[] source, int[] target) {
        Set<Long> set = new HashSet<>();
        for (int[] block : blocked) set.add(block[0] * 1001L + block[1]);
        return dfs(source[0], source[1], set, source, target, new HashSet<>()) && dfs(target[0], target[1], set, target, source, new HashSet<>());
    }
    
    private boolean dfs(int x, int y, Set<Long> set, int[] source, int[] target, Set<Long> visited) {
        if (x == target[0] && y == target[1]) return true;
        long hash = x * 1001L + y;
        if (set.contains(hash) || visited.contains(hash) || x < 0 || x > 999_999 || y < 0 || y > 999_999) return false;
        if (Math.abs(source[0] - x) + Math.abs(source[1] - y) > 200) return true;
        visited.add(hash);
        return dfs(x + 1, y, set, source, target, visited) || dfs(x - 1, y, set, source, target, visited) || dfs(x, y + 1, set, source, target, visited) || dfs(x, y - 1, set, source, target, visited);
    }
}
```

__Python__:

```Python
class Solution:
    def isEscapePossible(self, blocked: List[List[int]], source: List[int], target: List[int]) -> bool:
        blocked = set(map(tuple, blocked))
        
        def dfs(x: int, y: int, target: List[int], seen: Set[tuple]) -> bool:
            if not (0 <= x < 10 ** 6 and 0 <= y < 10 ** 6) or (x, y) in blocked or (x, y) in seen: 
                return False
            seen.add((x, y))
            if len(seen) > 20000 or [x, y] == target: 
                return True
            return dfs(x - 1, y, target, seen) or dfs(x, y - 1, target, seen) or dfs(x + 1, y, target, seen) or dfs(x, y + 1, target, seen)
        return dfs(source[0], source[1], target, set()) and dfs(target[0], target[1], source, set())
```
