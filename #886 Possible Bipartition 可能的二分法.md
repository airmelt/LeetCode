# 886 Possible Bipartition 可能的二分法

__Description__:
We want to split a group of n people (labeled from 1 to n) into two groups of any size. Each person may dislike some other people, and they should not go into the same group.

Given the integer n and the array dislikes where dislikes[i] = [ai, bi] indicates that the person labeled ai does not like the person labeled bi, return true if it is possible to split everyone into two groups in this way.

__Example:__

Example 1:

Input: n = 4, dislikes = [[1,2],[1,3],[2,4]]
Output: true
Explanation: group1 [1,4] and group2 [2,3].

Example 2:

Input: n = 3, dislikes = [[1,2],[1,3],[2,3]]
Output: false

Example 3:

Input: n = 5, dislikes = [[1,2],[2,3],[3,4],[4,5],[1,5]]
Output: false

__Constraints:__

1 <= n <= 2000
0 <= dislikes.length <= 10^4
dislikes[i].length == 2
1 <= dislikes[i][j] <= n
ai < bi
All the pairs of dislikes are unique.

__题目描述__:
给定一组 N 人（编号为 1, 2, ..., N）， 我们想把每个人分进任意大小的两组。

每个人都可能不喜欢其他人，那么他们不应该属于同一组。

形式上，如果 dislikes[i] = [a, b]，表示不允许将编号为 a 和 b 的人归入同一组。

当可以用这种方法将所有人分进两组时，返回 true；否则返回 false。

__示例 :__

示例 1：

输入：N = 4, dislikes = [[1,2],[1,3],[2,4]]
输出：true
解释：group1 [1,4], group2 [2,3]

示例 2：

输入：N = 3, dislikes = [[1,2],[1,3],[2,3]]
输出：false

示例 3：

输入：N = 5, dislikes = [[1,2],[2,3],[3,4],[4,5],[1,5]]
输出：false

__提示:__

1 <= N <= 2000
0 <= dislikes.length <= 10000
dislikes[i].length == 2
1 <= dislikes[i][j] <= N
dislikes[i][0] < dislikes[i][1]
对于 dislikes[i] == dislikes[j] 不存在 i != j

__思路__:

DFS ➕ 染色
给定 color 数组, 0 表示未染色, 正负 1 表示两种不同的颜色
在 DFS 过程中, 如果相邻结点颜色相同, 则返回 false
检查完所有人返回 true
时间复杂度为 O(n + m), 空间复杂度为 O(n + m), 其中 m 为 dislikes 数组的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool possibleBipartition(int n, vector<vector<int>>& dislikes) 
    {
        vector<vector<int>> graph(n + 1);
        for (const auto& d : dislikes)
        {
            graph[d[0]].emplace_back(d[1]);
            graph[d[1]].emplace_back(d[0]);
        }
        vector<int> color(n + 1,0);
        function<bool(int, int)> dfs = [&](int cur_node, int col)
        {
            if (color[cur_node]) return color[cur_node] == col;
            color[cur_node] = col;
            for (const auto& next_node : graph[cur_node]) if(!dfs(next_node, -col)) return false;
            return true;
        };
        for (int i = 1; i <= n; i++) if (!color[i] and !dfs(i, 1)) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean possibleBipartition(int n, int[][] dislikes) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i <= n; i++) graph.add(new ArrayList<Integer>());
        for (int d[] : dislikes) {
            graph.get(d[0]).add(d[1]);
            graph.get(d[1]).add(d[0]);
        }
        int color[] = new int[n + 1];
        for (int i = 1; i <= n; i++) if (color[i] == 0 && !dfs(i, 1, color, graph)) return false;
        return true;
    }
    
    private boolean dfs(int curNode, int col, int[] color, List<List<Integer>> graph) {
        if (color[curNode] != 0) return color[curNode] == col;
        color[curNode] = col;
        for (int nextNode : graph.get(curNode)) if(!dfs(nextNode, -col, color, graph)) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def possibleBipartition(self, n: int, dislikes: List[List[int]]) -> bool:
        graph, color = defaultdict(list), [0] * (n + 1)
        for d in dislikes:
            graph[d[0]].append(d[1])
            graph[d[1]].append(d[0])
            
        def dfs(cur_node: int, col: int) -> bool:
            if color[cur_node]:
                return color[cur_node] == col
            color[cur_node] = col
            for next_node in graph[cur_node]:
                if not dfs(next_node, -col):
                    return False
            return True
        for i in range(1, n + 1):
            if not color[i] and not dfs(i, 1):
                return False
        return True
```
