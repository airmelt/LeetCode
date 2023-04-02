# 1584 Min Cost to Connect All Points 连接所有点的最小费用

__Description:__

You are given an array `points` representing integer coordinates of some points on a 2D-plane, where `points[i] = [xi, yi]`.

The cost of connecting two points `[xi, yi]` and `[xj, yj]` is the __manhattan distance__ between them: `|xi - xj| + |yi - yj|`, where `|val|` denotes the absolute value of `val`.

Return the minimum cost to make all points connected. All points are connected if there is exactly one simple path between any two points.

__Example:__

Example 1:

![1584-1](https://assets.leetcode.com/uploads/2020/08/26/d.png)

```text
Input: points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
Output: 20
Explanation: 

We can connect the points as shown above to get the minimum cost of 20.
Notice that there is a unique path between every pair of points.
```

![1584-2](https://assets.leetcode.com/uploads/2020/08/26/c.png)

Example 2:

```text
Input: points = [[3,12],[-2,5],[-4,1]]
Output: 18
```

__Constraints:__

- `1 <= points.length <= 1000`
- `-10 ^ 6 <= xi, yi <= 10 ^ 6`
- All pairs `(xi, yi)` are distinct.

__题目描述:__

给你一个 `points` 数组，表示 2D 平面上的一些点，其中 `points[i] = [xi, yi]` 。

连接点 `[xi, yi]` 和点 `[xj, yj]` 的费用为它们之间的 __曼哈顿距离__ : `|xi - xj| + |yi - yj|` ，其中 `|val|` 表示 `val` 的绝对值。

请你返回将所有点连接的最小总费用。只有任意两点之间 有且仅有 一条简单路径时，才认为所有点都已连接。

__示例:__

示例 1：

![1584-3](https://assets.leetcode.com/uploads/2020/08/26/d.png)

```text
输入：points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
输出：20
解释：

我们可以按照上图所示连接所有点得到最小总费用，总费用为 20 。
注意到任意两个点之间只有唯一一条路径互相到达。
```

![1584-4](https://assets.leetcode.com/uploads/2020/08/26/c.png)

示例 2：

```text
输入：points = [[3,12],[-2,5],[-4,1]]
输出：18
```

示例 3：

```text
输入：points = [[0,0],[1,1],[1,0],[-1,1]]
输出：4
```

示例 4：

```text
输入：points = [[-1000000,-1000000],[1000000,1000000]]
输出：4000000
```

示例 5：

```text
输入：points = [[0,0]]
输出：0
```

__提示：__

- `1 <= points.length <= 1000`
- `-10 ^ 6 <= xi, yi <= 10 ^ 6`
- 所有点 `(xi, yi)` 两两不同。

__思路:__

```text
1. Prim 算法
从 n 个点中选择一个点作为起点
然后从剩下的点中选择与起点距离最近的点
接着重复上一步, 每次选择距离最近的点并更新选择过的点
直到把所有点加入
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
2. Kruskal 算法
每次选择最短的边
如果当前最短的边会连成环, 则选择倒数第二短的边
直到将所有点加入同一个连通分量
可以用并查集判断是否成环
时间复杂度为 O(N ^ 2logN), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class DisjointSetUnion 
{
private:
    vector<int> f, rank;
    int n;
public:
    DisjointSetUnion(int _n) 
    {
        n = _n;
        rank.resize(n, 1);
        f.resize(n);
        for (int i = 0; i < n; i++) f[i] = i;
    }

    int find(int x) 
    {
        return f[x] == x ? x : f[x] = find(f[x]);
    }

    int unionSet(int x, int y) 
    {
        int fx = find(x), fy = find(y);
        if (fx == fy) return false;
        if (rank[fx] < rank[fy]) swap(fx, fy);
        rank[fx] += rank[fy];
        f[fy] = fx;
        return true;
    }
};

class BIT 
{
public:
    vector<int> tree, idRec;
    int n;

    BIT(int _n) 
    {
        n = _n;
        tree.resize(n, INT_MAX);
        idRec.resize(n, -1);
    }

    int lowbit(int k) 
    {
        return k & (-k);
    }

    void update(int pos, int val, int id) 
    {
        while (pos > 0) 
        {
            if (tree[pos] > val) 
            {
                tree[pos] = val;
                idRec[pos] = id;
            }
            pos -= lowbit(pos);
        }
    }

    int query(int pos) 
    {
        int minval = INT_MAX, j = -1;
        while (pos < n) 
        {
            if (minval > tree[pos]) 
            {
                minval = tree[pos];
                j = idRec[pos];
            }
            pos += lowbit(pos);
        }
        return j;
    }
};

struct Edge 
{
    int len, x, y;
    Edge(int len, int x, int y) : len(len), x(x), y(y) {}
    bool operator<(const Edge& a) const
    {
        return len < a.len;
    }
};

struct Pos 
{
    int id, x, y;
    bool operator<(const Pos& a) const 
    {
        return x == a.x ? y < a.y : x < a.x;
    }
};

class Solution 
{
public:
    vector<Edge> edges;
    vector<Pos> pos;

    void build(int n) 
    {
        sort(pos.begin(), pos.end());
        vector<int> a(n), b(n);
        for (int i = 0; i < n; i++) 
        {
            a[i] = pos[i].y - pos[i].x;
            b[i] = pos[i].y - pos[i].x;
        }
        sort(b.begin(), b.end());
        b.erase(unique(b.begin(), b.end()), b.end());
        int num = b.size();
        BIT bit(num + 1);
        for (int i = n - 1; i >= 0; i--) 
        {
            int poss = lower_bound(b.begin(), b.end(), a[i]) - b.begin() + 1;
            int j = bit.query(poss);
            if (j != -1) edges.emplace_back(abs(pos[i].x - pos[j].x) + abs(pos[i].y - pos[j].y), pos[i].id, pos[j].id);
            bit.update(poss, pos[i].x + pos[i].y, i);
        }
    }

    void solve(vector<vector<int>>& points, int n) 
    {
        pos.resize(n);
        for (int i = 0; i < n; i++) 
        {
            pos[i].x = points[i][0];
            pos[i].y = points[i][1];
            pos[i].id = i;
        }
        build(n);
        for (int i = 0; i < n; i++) swap(pos[i].x, pos[i].y);
        build(n);
        for (int i = 0; i < n; i++) pos[i].x = -pos[i].x;
        build(n);
        for (int i = 0; i < n; i++) swap(pos[i].x, pos[i].y);
        build(n);
    }

    int minCostConnectPoints(vector<vector<int>>& points) 
    {
        int n = points.size();
        solve(points, n);
        DisjointSetUnion dsu(n);
        sort(edges.begin(), edges.end());
        int result = 0, num = 1;
        for (auto& [len, x, y] : edges) 
        {
            if (dsu.unionSet(x, y)) 
            {
                result += len;
                ++num;
                if (num == n) break;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minCostConnectPoints(int[][] points) {
        return prim(points, 0);
    }
    
    private int prim(int[][] points, int start) {
        int n = points.length, result = 0, graph[][] = new int[n][n], visited[] = new int[n], minCost[] = new int[n];
        Arrays.fill(minCost, Integer.MAX_VALUE);
        visited[start] = 1;
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) graph[i][j] = graph[j][i] = Math.abs(points[i][0] - points[j][0]) + Math.abs(points[i][1] - points[j][1]);
        for (int i = 0; i < n; i++) {
            if (i == start) continue;
            minCost[i] = graph[i][start];
        }
        for (int i = 1; i < n; i++) {
            int minIdx = -1, minVal = Integer.MAX_VALUE;
            for (int j = 0; j < n; j++) {
                if (visited[j] == 0 && minCost[j] < minVal) {
                    minIdx = j;
                    minVal = minCost[j];
                }
            }
            visited[minIdx] = 1;
            result += minVal;
            for (int j = 0; j < n; j++) if (visited[j] == 0 && graph[j][minIdx] < minCost[j]) minCost[j] = graph[j][minIdx];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        n = len(points)
        costs = [[abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1]) for i in range(n)] for j in range(n)]
        
        def prim(graph: List[List[int]]):
            dis, pre, flag, k = [graph[0][i] for i in range(n)], [0] * n, [True] + [False] * (n - 1), 0
            for j in range(n - 1):
                mini = float('inf')
                for i in range(n):
                    if mini > dis[i] and not flag[i]:
                        mini = dis[i]
                        k = i
                if not k:
                    return
                flag[k] = True
                for i in range(n):
                    if dis[i] > graph[k][i] and not flag[i]:
                        dis[i], pre[i] = graph[k][i], k
            return dis
        return sum(prim(costs))
```
