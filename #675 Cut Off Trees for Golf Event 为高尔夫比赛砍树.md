# 675 Cut Off Trees for Golf Event 为高尔夫比赛砍树

__Description__:
You are asked to cut off all the trees in a forest for a golf event. The forest is represented as an m x n matrix. In this matrix:

0 means the cell cannot be walked through.
1 represents an empty cell that can be walked through.
A number greater than 1 represents a tree in a cell that can be walked through, and this number is the tree's height.
In one step, you can walk in any of the four directions: north, east, south, and west. If you are standing in a cell with a tree, you can choose whether to cut it off.

You must cut off the trees in order from shortest to tallest. When you cut off a tree, the value at its cell becomes 1 (an empty cell).

Starting from the point (0, 0), return the minimum steps you need to walk to cut off all the trees. If you cannot cut off all the trees, return -1.

You are guaranteed that no two trees have the same height, and there is at least one tree needs to be cut off.

__Example:__

Example 1:

![trees1](https://assets.leetcode.com/uploads/2020/11/26/trees1.jpg)

Input: forest = [[1,2,3],[0,0,4],[7,6,5]]
Output: 6
Explanation: Following the path above allows you to cut off the trees from shortest to tallest in 6 steps.

Example 2:

![trees2](https://assets.leetcode.com/uploads/2020/11/26/trees2.jpg)

Input: forest = [[1,2,3],[0,0,0],[7,6,5]]
Output: -1
Explanation: The trees in the bottom row cannot be accessed as the middle row is blocked.

Example 3:

Input: forest = [[2,3,4],[0,0,5],[8,7,6]]
Output: 6
Explanation: You can follow the same path as Example 1 to cut off all the trees.
Note that you can cut off the first tree at (0, 0) before making any steps.

__Constraints:__

m == forest.length
n == forest[i].length
1 <= m, n <= 50
0 <= forest[i][j] <= 10^9

__题目描述__:
你被请来给一个要举办高尔夫比赛的树林砍树。树林由一个 m x n 的矩阵表示， 在这个矩阵中：

0 表示障碍，无法触碰
1 表示地面，可以行走
比 1 大的数 表示有树的单元格，可以行走，数值表示树的高度
每一步，你都可以向上、下、左、右四个方向之一移动一个单位，如果你站的地方有一棵树，那么你可以决定是否要砍倒它。

你需要按照树的高度从低向高砍掉所有的树，每砍过一颗树，该单元格的值变为 1（即变为地面）。

你将从 (0, 0) 点开始工作，返回你砍完所有树需要走的最小步数。 如果你无法砍完所有的树，返回 -1 。

可以保证的是，没有两棵树的高度是相同的，并且你至少需要砍倒一棵树。

__示例 :__

示例 1：

![树1](https://assets.leetcode.com/uploads/2020/11/26/trees1.jpg)

输入：forest = [[1,2,3],[0,0,4],[7,6,5]]
输出：6
解释：沿着上面的路径，你可以用 6 步，按从最矮到最高的顺序砍掉这些树。

示例 2：

![树2](https://assets.leetcode.com/uploads/2020/11/26/trees2.jpg)
输入：forest = [[1,2,3],[0,0,0],[7,6,5]]
输出：-1
解释：由于中间一行被障碍阻塞，无法访问最下面一行中的树。

示例 3：

输入：forest = [[2,3,4],[0,0,5],[8,7,6]]
输出：6
解释：可以按与示例 1 相同的路径来砍掉所有的树。
(0,0) 位置的树，可以直接砍去，不用算步数。

__提示:__

m == forest.length
n == forest[i].length
1 <= m, n <= 50
0 <= forest[i][j] <= 10^9

__思路__:

BFS
先将各树按照树的高度排序
然后用 BFS 搜索两相邻树之间的距离
求出距离之和, 只要不能去到任何一棵树, 直接返回 -1
时间复杂度 O(m ^ 2 * n ^ 2), 空间复杂度 O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int cutOffTree(vector<vector<int>>& forest) 
    {
        m = forest.size();
        n = forest[0].size();
        vector<tuple<int, int, int>> trees;
        int result = 0, sc = 0, sr = 0, v = 0;
        for (int r = 0; r < m; ++r) for (int c = 0; c < n; ++c) if ((v = forest[r][c]) > 1) trees.emplace_back(v, r, c);
        sort(trees.begin(), trees.end());
        for (const auto &tree: trees) 
        {
            int d = bfs(forest, sr, sc, get<1>(tree), get<2>(tree));
            if (d < 0) return -1;
            result += d;
            sr = get<1>(tree); 
            sc = get<2>(tree);
        }
        return result;
    }
private:
    int m, n;
    
    int bfs(vector<vector<int>>& forest, int sr, int sc, int tr, int tc) 
    {
        int dx[]{ -1, 1, 0, 0}, dy[]{ 0, 0, -1, 1 };
        queue<tuple<int, int, int>> q;
        q.emplace(sr, sc, 0);
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        visited[sr][sc] = true;
        while (!q.empty()) 
        {
            auto cur = q.front();
            q.pop();
            if (get<0>(cur) == tr && get<1>(cur) == tc) return get<2>(cur);
            for (int i = 0; i < 4; i++) 
            {
                int x = get<0>(cur) + dx[i];
                int y = get<1>(cur) + dy[i];
                if (-1 < x and x < m and -1 < y and y < n and !visited[x][y] and forest[x][y] > 0) 
                {
                    visited[x][y] = true;
                    q.emplace(x, y, get<2>(cur) + 1);
                }
            }
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    private int m = 0, n = 0, dx[] = {-1, 1, 0, 0}, dy[] = {0, 0, -1, 1};

    public int cutOffTree(List<List<Integer>> forest) {
        m = forest.size();
        n = forest.get(0).size();
        List<int[]> trees = new ArrayList();
        for (int r = 0; r < m; ++r) for (int c = 0; c < n; ++c) if (forest.get(r).get(c) > 1) trees.add(new int[]{ forest.get(r).get(c), r, c });
        Collections.sort(trees, (a, b) -> Integer.compare(a[0], b[0]));
        int result = 0, sr = 0, sc = 0;
        for (int[] tree: trees) {
            int d = bfs(forest, sr, sc, tree[1], tree[2]);
            if (d < 0) return -1;
            result += d;
            sr = tree[1]; 
            sc = tree[2];
        }
        return result;
    }
    
    public int bfs(List<List<Integer>> forest, int sr, int sc, int tr, int tc) {
        Queue<int[]> queue = new LinkedList();
        queue.add(new int[]{ sr, sc, 0 });
        boolean[][] visited = new boolean[m][n];
        visited[sr][sc] = true;
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            if (cur[0] == tr && cur[1] == tc) return cur[2];
            for (int i = 0; i < 4; i++) {
                int x = cur[0] + dx[i];
                int y = cur[1] + dy[i];
                if (-1 < x && x < m && -1 < y && y < n && !visited[x][y] && forest.get(x).get(y) > 0) {
                    visited[x][y] = true;
                    queue.add(new int[]{ x, y, cur[2] + 1 });
                }
            }
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def cutOffTree(self, forest):
        trees, m, n, sr, sc, result = sorted((v, r, c) for r, row in enumerate(forest) for c, v in enumerate(row) if v > 1), len(forest), len(forest[0]), 0, 0, 0
        
        def bfs(sr: int, sc: int, tr: int, tc: int) -> int:
            queue, visited = collections.deque([(sr, sc, 0)]), {(sr, sc)}
            while queue:
                r, c, d = queue.popleft()
                if r == tr and c == tc:
                    return d
                for x, y in ((r - 1, c), (r + 1, c), (r, c - 1), (r, c + 1)):
                    if (-1 < x < m and -1 < y < n and (x, y) not in visited and forest[x][y]):
                        visited.add((x, y))
                        queue.append((x, y, d + 1))
            return -1
        
        sr = sc = result = 0
        for _, tr, tc in trees:
            d = bfs(sr, sc, tr, tc)
            if d < 0: 
                return -1
            result += d
            sr, sc = tr, tc
        return result
```
