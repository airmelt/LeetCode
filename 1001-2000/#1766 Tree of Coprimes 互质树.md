# 1766 Tree of Coprimes 互质树

__Description:__

There is a tree (i.e., a connected, undirected graph that has no cycles) consisting of `n` nodes numbered from `0` to `n - 1` and exactly `n - 1` edges. Each node has a value associated with it, and the __root__ of the tree is node `0`.

To represent this tree, you are given an integer array `nums` and a 2D array `edges`. Each `nums[i]` represents the `i ^ th` node's value, and each `edges[j] = [uj, vj]` represents an edge between nodes `uj` and `vj` in the tree.

Two values `x` and `y` are __coprime__ if `gcd(x, y) == 1` where `gcd(x, y)` is the __greatest common divisor__ of `x` and `y`.

An ancestor of a node `i` is any other node on the shortest path from node `i` to the __root__. A node is __not__ considered an ancestor of itself.

Return _an array_ `ans` _of size_ `n`, _where_ `ans[i]` _is the closest ancestor to node_ `i` _such that_ `nums[i]` _and_ `nums[ans[i]]` are __coprime__, or `-1` _if there is no such ancestor_.

__Example:__

Example 1:

![1766-1](https://assets.leetcode.com/uploads/2021/01/06/untitled-diagram.png)

```text
Input: nums = [2,3,3,2], edges = [[0,1],[1,2],[1,3]]
Output: [-1,0,0,1]
Explanation: In the above figure, each node's value is in parentheses.
- Node 0 has no coprime ancestors.
- Node 1 has only one ancestor, node 0. Their values are coprime (gcd(2,3) == 1).
- Node 2 has two ancestors, nodes 1 and 0. Node 1's value is not coprime (gcd(3,3) == 3), but node 0's
  value is (gcd(2,3) == 1), so node 0 is the closest valid ancestor.
- Node 3 has two ancestors, nodes 1 and 0. It is coprime with node 1 (gcd(3,2) == 1), so node 1 is its
  closest valid ancestor.
```

Example 2:

![1766-2](https://assets.leetcode.com/uploads/2021/01/06/untitled-diagram1.png)

```text
Input: nums = [5,6,10,2,3,6,15], edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]]
Output: [-1,0,-1,0,0,0,-1]
```

__Constraints:__

- `nums.length == n`
- `1 <= nums[i] <= 50`
- `1 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[j].length == 2`
- `0 <= uj, vj < n`
- `uj != vj`

__题目描述:__

给你一个 `n` 个节点的树（也就是一个无环连通无向图），节点编号从 `0` 到 `n - 1` ，且恰好有 `n - 1` 条边，每个节点有一个值。树的 __根节点__ 为 0 号点。

给你一个整数数组 `nums` 和一个二维数组 `edges` 来表示这棵树。 `nums[i]` 表示第 `i` 个点的值， `edges[j] = [uj, vj]` 表示节点 `uj` 和节点 `vj` 在树中有一条边。

当 `gcd(x, y) == 1` ，我们称两个数 `x` 和 `y` 是 __互质的__ ，其中 `gcd(x, y)` 是 `x` 和 `y` 的 __最大公约数__ 。

从节点 `i` 到 __根__ 最短路径上的点都是节点 `i` 的祖先节点。一个节点 __不是__ 它自己的祖先节点。

请你返回一个大小为 `n` 的数组 `ans` ，其中 `ans[i]`是离节点 `i` 最近的祖先节点且满足 `nums[i]` 和 `nums[ans[i]]` 是 __互质的__ ，如果不存在这样的祖先节点， `ans[i]` 为 `-1` 。

__示例:__

示例 1：

![1766-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/02/20/untitled-diagram.png)

```text
输入：nums = [2,3,3,2], edges = [[0,1],[1,2],[1,3]]
输出：[-1,0,0,1]
解释：上图中，每个节点的值在括号中表示。
- 节点 0 没有互质祖先。
- 节点 1 只有一个祖先节点 0 。它们的值是互质的（gcd(2,3) == 1）。
- 节点 2 有两个祖先节点，分别是节点 1 和节点 0 。节点 1 的值与它的值不是互质的（gcd(3,3) == 3）但节点 0 的值是互质的(gcd(2,3) == 1)，所以节点 0 是最近的符合要求的祖先节点。
- 节点 3 有两个祖先节点，分别是节点 1 和节点 0 。它与节点 1 互质（gcd(3,2) == 1），所以节点 1 是离它最近的符合要求的祖先节点。
```

示例 2：

![1766-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/02/20/untitled-diagram1.png)

```text
输入：nums = [5,6,10,2,3,6,15], edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]]
输出：[-1,0,-1,0,0,0,-1]
```

__提示：__

- `nums.length == n`
- `1 <= nums[i] <= 50`
- `1 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[j].length == 2`
- `0 <= uj, vj < n`
- `uj != vj`

__思路:__

```text
DFS
先用一个数组记录下 50 以内的互质数
然后用一个数组记录下每个结点的临接结点
用一个数组记录下每个结点的深度
每次遍历的时候, 从互质结点中选择最近的祖父结点
使用 pos 记录当前结点对应的编号
注意遍历的时候, 将记录的 pos 信息清除
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> getCoprimes(vector<int>& nums, vector<vector<int>>& edges) 
    {
        int n = nums.size();
        graph.resize(n);
        level.resize(n);
        prime_pair.resize(51);
        pos = vector<int>(52, -1);
        result = vector<int>(n, -1);       
        for (const auto& edge : edges)
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        for (int i = 1; i < 51; i++) for (int j = 1; j < 51; j++) if (__gcd(i, j) == 1) prime_pair[i].emplace_back(j);
        dfs(nums, 0, -1);
        return result;
    }
private:
    vector<vector<int>> graph;
    vector<vector<int>> prime_pair;
    vector<int> result;
    vector<int> pos;
    vector<int> level;

    void dfs(vector<int>& nums, int u, int father)
    {
        int t = nums[u];
        for (int v : prime_pair[t]) 
        {
            if (pos[v] == -1) continue;
            if (result[u] == -1 or level[result[u]] < level[pos[v]]) result[u] = pos[v];
        }
        int p = pos[t];
        pos[t] = u;
        for (int i : graph[u]) 
        {
            if (i == father) continue;
            level[i] = level[u] + 1;
            dfs(nums, i, u);
        }
        pos[t] = p;
    }
};
```

__Java__:

```Java
class Solution {
    private int[] result;
    private int[] level;
    private int[] pos = new int[52];
    private Map<Integer, List<Integer>> graph = new HashMap<>();
    private Map<Integer, List<Integer>> primePair = new HashMap<>();

    public int[] getCoprimes(int[] nums, int[][] edges) {
        int n = nums.length;
        result = new int[n];
        level = new int[n];
        Arrays.fill(result, -1);
        Arrays.fill(pos, -1);
        for (int[] edge : edges) {
            List<Integer> list1 = graph.getOrDefault(edge[0], new ArrayList<>());
            list1.add(edge[1]);
            graph.put(edge[0], list1);
            List<Integer> list2 = graph.getOrDefault(edge[1], new ArrayList<>());
            list2.add(edge[0]);
            graph.put(edge[1], list2);
        }
        for (int i = 1; i < 51; i++) {
            for (int j = 1; j < 51; j++) {
                if (gcd(i, j) == 1) {
                    List<Integer> list = primePair.getOrDefault(i, new ArrayList<>());
                    list.add(j);
                    primePair.put(i, list);
                }
            }
        }
        dfs(nums, 0, -1);
        return result;
    }

    private void dfs(int[] nums, int u, int father) {
        int t = nums[u];
        for (int v : primePair.get(t)) {
            if (pos[v] == -1) continue;
            if (result[u] == -1 || level[result[u]] < level[pos[v]]) result[u] = pos[v];
        }
        int p = pos[t];
        pos[t] = u;
        for (int i : graph.getOrDefault(u, new ArrayList<>())) {
            if (i == father) continue;
            level[i] = level[u] + 1;
            dfs(nums, i, u);
        }
        pos[t] = p;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
prime_pair = defaultdict(set)
for x in range(1, 51):
    for y in range(x, 51):
        if gcd(x, y) == 1:
            prime_pair[x].add(y)
            prime_pair[y].add(x)

class Solution:
    def getCoprimes(self, nums: List[int], edges: List[List[int]]) -> List[int]:
        graph, pre, visited, result = defaultdict(set), defaultdict(list), [True] + [False] * ((n := len(nums)) - 1), [-1] * n
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)

        def dfs(root: int, level: int) -> NoReturn:
            cur = recent = -1
            for num in prime_pair[nums[root]]:
                if pre[num] and pre[num][-1][1] > recent:
                    cur, recent = pre[num][-1]
            result[root] = cur
            pre[nums[root]].append([root, level])
            visited[root] = True
            for son in graph[root]:
                if not visited[son]:
                    dfs(son, level + 1)
            pre[nums[root]].pop()
            visited[root] = False
        dfs(0, 0)
        return result
```
