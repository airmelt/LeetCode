# 803 Bricks Falling When Hit 打砖块

__Description__:
You are given an m x n binary grid, where each 1 represents a brick and 0 represents an empty space. A brick is stable if:

It is directly connected to the top of the grid, or
At least one other brick in its four adjacent cells is stable.
You are also given an array hits, which is a sequence of erasures we want to apply. Each time we want to erase the brick at the location hits[i] = (rowi, coli). The brick on that location (if it exists) will disappear. Some other bricks may no longer be stable because of that erasure and will fall. Once a brick falls, it is immediately erased from the grid (i.e., it does not land on other stable bricks).

Return an array result, where each result[i] is the number of bricks that will fall after the ith erasure is applied.

Note that an erasure may refer to a location with no brick, and if it does, no bricks drop.

__Example:__

Example 1:

Input: grid = [[1,0,0,0],[1,1,1,0]], hits = [[1,0]]
Output: [2]
Explanation: Starting with the grid:
[[1,0,0,0],
 [1,1,1,0]]
We erase the underlined brick at (1,0), resulting in the grid:
[[1,0,0,0],
 [0,__1__,__1__,0]]
The two underlined bricks are no longer stable as they are no longer connected to the top nor adjacent to another stable brick, so they will fall. The resulting grid is:
[[1,0,0,0],
 [0,0,0,0]]
Hence the result is [2].

Example 2:

Input: grid = [[1,0,0,0],[1,1,0,0]], hits = [[1,1],[1,0]]
Output: [0,0]
Explanation: Starting with the grid:
[[1,0,0,0],
 [1,1,0,0]]
We erase the underlined brick at (1,1), resulting in the grid:
[[1,0,0,0],
 [1,0,0,0]]
All remaining bricks are still stable, so no bricks fall. The grid remains the same:
[[1,0,0,0],
 [1,0,0,0]]
Next, we erase the underlined brick at (1,0), resulting in the grid:
[[1,0,0,0],
 [0,0,0,0]]
Once again, all remaining bricks are still stable, so no bricks fall.
Hence the result is [0,0].

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m, n <= 200
grid[i][j] is 0 or 1.
1 <= hits.length <= 4 * 10^4
hits[i].length == 2
0 <= xi <= m - 1
0 <= yi <= n - 1
All (xi, yi) are unique.

__题目描述__:
有一个 m x n 的二元网格，其中 1 表示砖块，0 表示空白。砖块 稳定（不会掉落）的前提是：

一块砖直接连接到网格的顶部，或者
至少有一块相邻（4 个方向之一）砖块 稳定 不会掉落时
给你一个数组 hits ，这是需要依次消除砖块的位置。每当消除 hits[i] = (rowi, coli) 位置上的砖块时，对应位置的砖块（若存在）会消失，然后其他的砖块可能因为这一消除操作而掉落。一旦砖块掉落，它会立即从网格中消失（即，它不会落在其他稳定的砖块上）。

返回一个数组 result ，其中 result[i] 表示第 i 次消除操作对应掉落的砖块数目。

注意，消除可能指向是没有砖块的空白位置，如果发生这种情况，则没有砖块掉落。

__示例 :__

示例 1：

输入：grid = [[1,0,0,0],[1,1,1,0]], hits = [[1,0]]
输出：[2]
解释：
网格开始为：
[[1,0,0,0]，
 [1,1,1,0]]
消除 (1,0) 处加粗的砖块，得到网格：
[[1,0,0,0]
 [0,__1__,__1__,0]]
两个加粗的砖不再稳定，因为它们不再与顶部相连，也不再与另一个稳定的砖相邻，因此它们将掉落。得到网格：
[[1,0,0,0],
 [0,0,0,0]]
因此，结果为 [2] 。

示例 2：

输入：grid = [[1,0,0,0],[1,1,0,0]], hits = [[1,1],[1,0]]
输出：[0,0]
解释：
网格开始为：
[[1,0,0,0],
 [1,1,0,0]]
消除 (1,1) 处加粗的砖块，得到网格：
[[1,0,0,0],
 [1,0,0,0]]
剩下的砖都很稳定，所以不会掉落。网格保持不变：
[[1,0,0,0],
 [1,0,0,0]]
接下来消除 (1,0) 处加粗的砖块，得到网格：
[[1,0,0,0],
 [0,0,0,0]]
剩下的砖块仍然是稳定的，所以不会有砖块掉落。
因此，结果为 [0,0] 。

__提示:__

m == grid.length
n == grid[i].length
1 <= m, n <= 200
grid[i][j] 为 0 或 1
1 <= hits.length <= 4 * 10^4
hits[i].length == 2
0 <= xi <= m - 1
0 <= yi <= n - 1
所有 (xi, yi) 互不相同

__思路__:

1. DFS
参考[LeetCode #802 Find Eventual Safe States 找到最终的安全状态](https://www.jianshu.com/p/6b413124cb2d)
用一个标记遍历的状态
--grid[hits[0]][hits[1] 表示击中了砖块
2 表示稳定的砖块
倒推模拟打砖块, 即从 hits 的最后一个开始往回加砖块
打掉砖块之前周围 4 个方向都是不稳定的或者打之前就是 0, 不用处理
否则计算这个砖块与 1 的连通区域的大小, 这个大小减 1 就是这次掉落的砖块
时间复杂度为 O(mn + s), 空间复杂度为 O(mn), 其中 m, n 为 grid 的大小, s 为 hits 的大小
2. 并查集
将天花板看作所有砖块最初的父亲节点
倒推, 将每个结点尝试 merge 到一起, 统计连通区域的大小即可
时间复杂度为 O(mns), 空间复杂度为 O(mn), 其中 m, n 为 grid 的大小, s 为 hits 的大小

__代码__:
__C++__:

```C++
class UnionFind
{
private:
    unordered_map<int,int> father;
    unordered_map<int,int> size_of_set;
public:
    int find(int x)
    {
        int root = x;
        while (father[root] != INT_MAX) root = father[root];
        while(x != root) 
        {
            int original_father = father[x];
            father[x] = root;
            x = original_father;
        }
        return root;
    }
    
    int get_size_of_set(int x)
    {
        return size_of_set[find(x)];
    }
    
    bool is_connected(int x, int y)
    {
        return find(x) == find(y);
    }
    
    void merge(int x,int y)
    {
        auto root_x = find(x);
        auto root_y = find(y);
        if (root_x != root_y)
        {
            father[root_x] = root_y;
            size_of_set[root_y] += size_of_set[root_x];
            size_of_set.erase(root_x);
        }
    }
    
    void add(int x)
    {
        if (!father.count(x))
        {
            father[x] = INT_MAX;
            size_of_set[x] = 1;
        }
    }
};

class Solution 
{
private:
    static const int CEILING = -1;
    vector<pair<int,int>> d = { { 1, 0 }, { -1, 0 }, { 0, 1 }, { 0, -1 } };
public:
    void initialize(UnionFind& uf, vector<vector<int>>& grid, const vector<vector<int>>& hits, const int& m, const int& n)
    {
        uf.add(CEILING);
        for (const auto& hit : hits) --grid[hit.front()][hit.back()];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] == 1) uf.add(i * n + j);
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] == 1) merge_neighbors(uf, grid, i, j, m, n);
        for (int j = 0; j < n; j++) if(grid[0][j] == 1) uf.merge(j, CEILING);
    }
    
    bool is_valid(const int& i, const int& j, vector<vector<int>>& grid, const int& m, const int& n)
    {
        return -1 < i and i < m and -1 < j and j < n and grid[i][j] == 1;
    }
    
    void merge_neighbors(UnionFind& uf, vector<vector<int>>& grid, const int& i, const int& j, const int& m, const int& n)
    {
        for (const auto& dir : d)
        {
            int next_x = i + dir.first, next_y = j + dir.second;
            if (is_valid(next_x, next_y, grid, m, n)) uf.merge(next_x * n + next_y, i * n + j);
        }
    }
    
    vector<int> hitBricks(vector<vector<int>>& grid, vector<vector<int>>& hits) 
    {
        UnionFind uf;
        int m = grid.size(), n = grid[0].size();
        initialize(uf, grid, hits, m, n);
        vector<int> result(hits.size(), 0);
        for (int i = hits.size() - 1; i >= 0; i--)
        {
            int x = hits[i].front(), y = hits[i].back();
            if (++grid[x][y] != 1) continue;
            int after_hit = uf.get_size_of_set(CEILING);
            uf.add(x * n + y);
            merge_neighbors(uf,grid,x,y,m,n);
            if (!x) uf.merge(y, CEILING);
            if (uf.is_connected(x * n + y, CEILING)) result[i] = uf.get_size_of_set(CEILING) - after_hit - 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        List<Integer> result = new ArrayList<>();
        int n = graph.length, tag[] = new int[n];
        for (int i = 0; i < n; i++) if (dfs(graph, tag, i) == 2) result.add(i);
        return result;
    }
    
    private int dfs(int[][] graph, int[] tag, int start) {
        if (tag[start] != 0) return tag[start] == 2 ? 2 : 3;
        tag[start] = 1;
        for (int i : graph[start]) if (dfs(graph, tag, i) == 3) return (tag[i] = 3);
        return (tag[start] = 2);
    }
}
```

__Python__:

```Python
class Solution:
    def hitBricks(self, grid: List[List[int]], hits: List[List[int]]) -> List[int]:
        m, n, result = len(grid), len(grid[0]), [0] * (s := len(hits))
        
        def dfs(x: int, y: int) -> int:
            if -1 < x < m and -1 < y < n and grid[x][y] == 1:
                grid[x][y] = 2
                return 1 + dfs(x + 1, y) + dfs(x - 1, y) + dfs(x, y + 1) + dfs(x, y - 1)
            return 0
        
        def is_stable(x: int, y: int) -> bool:
            return x == 0 or (x + 1 < m and grid[x + 1][y] == 2) or (x - 1 >= 0 and grid[x - 1][y] == 2) or (y + 1 < n and grid[x][y + 1] == 2) or (y - 1 >= 0 and grid[x][y - 1] == 2)
        
        for x, y in hits:
            grid[x][y] -= 1
        for y in range(n):
            dfs(0, y)
        for i in range(s - 1, -1,-1):
            x, y = hits[i]
            grid[x][y] += 1
            if is_stable(x, y) and grid[x][y]:
                result[i] = dfs(x, y) - 1
        return result
```
