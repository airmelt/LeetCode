__Description__:
A tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.

Given a tree of n nodes labelled from 0 to n - 1, and an array of n - 1 edges where edges[i] = [ai, bi] indicates that there is an undirected edge between the two nodes ai and bi in the tree, you can choose any node of the tree as the root. When you select a node x as the root, the result tree has height h. Among all possible rooted trees, those with minimum height (i.e. min(h))  are called minimum height trees (MHTs).

Return a list of all MHTs' root labels. You can return the answer in any order.

The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

__Example:__
Example 1:
![Trees 1](https://upload-images.jianshu.io/upload_images/16639143-c928b8e86bb95ffc.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: n = 4, edges = [[1,0],[1,2],[1,3]]
Output: [1]
Explanation: As shown, the height of the tree is 1 when the root is the node with label 1 which is the only MHT.

Example 2:
![Trees 2](https://upload-images.jianshu.io/upload_images/16639143-342c40bd63e087cd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: n = 6, edges = [[3,0],[3,1],[3,2],[3,4],[5,4]]
Output: [3,4]

Example 3:

Input: n = 1, edges = []
Output: [0]

Example 4:

Input: n = 2, edges = [[0,1]]
Output: [0,1]
 
__Constraints:__

1 <= n <= 2 * 104
edges.length == n - 1
0 <= ai, bi < n
ai != bi
All the pairs (ai, bi) are distinct.
The given input is guaranteed to be a tree and there will be no repeated edges.

__题目描述__:
对于一个具有树特征的无向图，我们可选择任何一个节点作为根。图因此可以成为树，在所有可能的树中，具有最小高度的树被称为最小高度树。给出这样的一个图，写出一个函数找到所有的最小高度树并返回他们的根节点。

格式

该图包含 n 个节点，标记为 0 到 n - 1。给定数字 n 和一个无向边 edges 列表（每一个边都是一对标签）。

你可以假设没有重复的边会出现在 edges 中。由于所有的边都是无向边， [0, 1]和 [1, 0] 是相同的，因此不会同时出现在 edges 里。

__示例 :__
示例 1:

输入: n = 4, edges = [[1, 0], [1, 2], [1, 3]]
```
        0
        |
        1
       / \
      2   3 
```
输出: [1]

示例 2:

输入: n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]
```
     0  1  2
      \ | /
        3
        |
        4
        |
        5 
```
输出: [3, 4]

__说明:__
 根据树的定义，树是一个无向图，其中任何两个顶点只通过一条路径连接。 换句话说，一个任何没有简单环路的连通图都是一棵树。
树的高度是指根节点和叶子节点之间最长向下路径上边的数量。

__思路__:
拓扑排序
每一轮将度为 1的结点删除直到剩下最后一个或两个结点
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) 
    {
        vector<int> result;
        queue<int> q;
        vector<unordered_set<int>> graph(n);
        for (auto &edge : edges)
        {
            graph[edge[0]].insert(edge[1]);
            graph[edge[1]].insert(edge[0]);
        }
        for (int i = 0; i < n; i++) if (graph[i].size() == 1) q.push(i);
        while (n > 2)
        {
            int size = q.size();
            n -= size;
            while (size-- > 0)
            {
                int v = q.front();
                q.pop();
                for (auto &next : graph[v])
                {
                    graph[next].erase(v);
                    if (graph[next].size() == 1) q.push(next);
                }
            }
        }
        if (q.empty()) result.emplace_back(0);
        while (!q.empty())
        {
            result.emplace_back(q.front());
            q.pop();
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<Integer> result = new ArrayList<>(), queue = new ArrayList<>();
        boolean graph[][] = new boolean[n][n], visited[] = new boolean[n];
        int count = n, inDegree[] = new int[n];
        for (int[] edge : edges) {
            graph[edge[0]][edge[1]] = true;
            graph[edge[1]][edge[0]] = true;
            ++inDegree[edge[0]];
            ++inDegree[edge[1]];
        }
        while (count > 2) {
            findOuter(queue, inDegree);
            while (!queue.isEmpty()) {
                int v = queue.remove(0);
                --inDegree[v];
                visited[v] = true;
                --count;
                for (int i = 0; i < n; i++) {
                    if (graph[v][i]) {
                        graph[v][i] = false;
                        graph[i][v] = false;
                        --inDegree[i];
                    }
                }
            }
        }
        for (int i = 0; i < n; i++) if (!visited[i]) result.add(i);
        return result;
    }
    
    private void findOuter(List<Integer> queue, int[] inDegree) {
        for (int i = 0; i < inDegree.length; i++) if (inDegree[i] == 1) queue.add(i);
    }
}
```

__Python__:
```Python
class Solution:
    def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
        graph, in_degree, q, visited, count = [[False] * n for _ in range(n)], [0] * n, [], [False] * n, n
        for edge in edges:
            graph[edge[0]][edge[1]] = graph[edge[1]][edge[0]] = True
            in_degree[edge[0]] += 1
            in_degree[edge[1]] += 1
        while count > 2:
            q += [i for i in range(n) if in_degree[i] == 1]
            while q:
                v = q.pop(0)
                in_degree[v] -= 1
                count -= 1
                visited[v] = True
                for i in range(n):
                    if graph[v][i]:
                        in_degree[i] -= 1
                        graph[v][i] = graph[i][v] = False
        return [i for i in range(n) if not visited[i]]
```