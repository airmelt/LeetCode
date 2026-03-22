# 834 Sum of Distances in Tree 树中距离之和

__Description__:
There is an undirected connected tree with n nodes labeled from 0 to n - 1 and n - 1 edges.

You are given the integer n and the array edges where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the tree.

Return an array answer of length n where answer[i] is the sum of the distances between the ith node in the tree and all other nodes.

__Example:__

Example 1:

![lc-sumdist1](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist1.jpg)

Input: n = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
Output: [8,12,6,10,10,10]
Explanation: The tree is shown above.
We can see that dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5)
equals 1 + 1 + 2 + 2 + 2 = 8.
Hence, answer[0] = 8, and so on.

Example 2:

![lc-sumdist2](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist2.jpg)

Input: n = 1, edges = []
Output: [0]

Example 3:

![lc-sumdist3](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist3.jpg)

Input: n = 2, edges = [[1,0]]
Output: [1,1]

__Constraints:__

1 <= n <= 3 * 10^4
edges.length == n - 1
edges[i].length == 2
0 <= ai, bi < n
ai != bi
The given input represents a valid tree.

__题目描述__:
给定一个无向、连通的树。树中有 N 个标记为 0...N-1 的节点以及 N-1 条边 。

第 i 条边连接节点 edges[i][0] 和 edges[i][1] 。

返回一个表示节点 i 与其他所有节点距离之和的列表 ans。

__示例 :__

示例 1:

输入: N = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
输出: [8,12,6,10,10,10]
解释:
如下为给定的树的示意图：

```text
  0
 / \
1   2
   /|\
  3 4 5
```

我们可以计算出 dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5)
也就是 1 + 1 + 2 + 2 + 2 = 8。 因此，answer[0] = 8，以此类推。

__说明:__
1 <= N <= 10000

__思路__:

树形 DP
用一个数组 nodes 记录子结点数, 每个结点初始化为 1
dp[i] 表示到 i 各结点的距离之和, 每个结点初始化为 0
先用后序遍历, 得到所有的结点的子结点数
在后序遍历的同时, dp[i] 加上子结点的距离 dp[i] += dp[child] + nodes[child], 这样就完成了父结点到所有子结点的距离求和
然后还需要加上父结点到其他结点的距离和, 注意到这个时候已经求出了根结点的真正的距离和
因为已经求出来了子结点数, 所以某个结点的非子结点数是 n - nodes[i] - 1,
如果将父结点走到子结点改为从根结点出发 dp[child] = dp[root] - nodes[i] + (n - nodes[i])
dp[root] - nodes[i] 表示, 如果从根结点出发到当前结点需要少走 nodes[i] 的距离
n - nodes[i] 表示其他结点走到根结点再走到当前结点需要多走 1 步
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution
{
public:
    vector<int> sumOfDistancesInTree(int n, vector<vector<int>>& edges) 
    {
        vector<vector<int>> graph(n);
        vector<int> dp(n, 0), nodes(n, 1);
        for (const auto& edge : edges) 
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        post(0, -1, dp, nodes, graph);
        pre(0, -1, dp, nodes, graph, n);
        return dp;
    }
private:
    void post(int root, int parent, vector<int>& dp, vector<int>& nodes, vector<vector<int>>& graph) 
    {
        for (const auto& child : graph[root]) 
        {
            if (child == parent) continue;
            post(child, root, dp, nodes, graph);
            nodes[root] += nodes[child];
            dp[root] += dp[child] + nodes[child];
        }
    }
    
    void pre(int root, int parent, vector<int>& dp, vector<int>& nodes, vector<vector<int>>& graph, int n) {
        for (const auto& child : graph[root]) 
        {
            if (child == parent) continue;
            dp[child] = dp[root] + n - (nodes[child] << 1);
            pre(child, root, dp, nodes, graph, n);
        }
    }
};
```

__Java__:

```Java
class Solution {
    public int[] sumOfDistancesInTree(int n, int[][] edges) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
        int dp[] = new int[n], nodes[] = new int[n];
        Arrays.fill(nodes, 1);
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }
        post(0, -1, dp, nodes, graph);
        pre(0, -1, dp, nodes, graph, n);
        return dp;
    }
    
    private void post(int root, int parent, int[] dp, int[] nodes, List<List<Integer>> graph) {
        for (int child : graph.get(root)) {
            if (child == parent) continue;
            post(child, root, dp, nodes, graph);
            nodes[root] += nodes[child];
            dp[root] += dp[child] + nodes[child];
        }
    }
    
    private void pre(int root, int parent, int[] dp, int[] nodes, List<List<Integer>> graph, int n) {
        for (int child : graph.get(root)) {
            if (child == parent) continue;
            dp[child] = dp[root] + n - (nodes[child] << 1);
            pre(child, root, dp, nodes, graph, n);
        }
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfDistancesInTree(self, n: int, edges: List[List[int]]) -> List[int]:
        graph, dp, nodes = [list() for _ in range(n)], [0] * n, [1] * n
        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)
            
        def post(root: int, parent: int) -> None:
            for child in graph[root]:
                if child == parent:
                    continue
                post(child, root)
                nodes[root] += nodes[child]
                dp[root] += dp[child] + nodes[child]
                
        def pre(root: int, parent: int) -> None:
            for child in graph[root]:
                if child == parent:
                    continue
                dp[child] = dp[root] + n - (nodes[child] << 1)
                pre(child, root)
                
        post(0, -1)
        pre(0, -1)
        return dp
```
