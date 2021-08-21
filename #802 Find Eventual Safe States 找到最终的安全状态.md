# 802 Find Eventual Safe States 找到最终的安全状态

__Description__:
We start at some node in a directed graph, and every turn, we walk along a directed edge of the graph. If we reach a terminal node (that is, it has no outgoing directed edges), we stop.

We define a starting node to be safe if we must eventually walk to a terminal node. More specifically, there is a natural number k, so that we must have stopped at a terminal node in less than k steps for any choice of where to walk.

Return an array containing all the safe nodes of the graph. The answer should be sorted in ascending order.

The directed graph has n nodes with labels from 0 to n - 1, where n is the length of graph. The graph is given in the following form: graph[i] is a list of labels j such that (i, j) is a directed edge of the graph, going from node i to node j.

__Example:__

Example 1:

![graph](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png)

Input: graph = [[1,2],[2,3],[5],[0],[5],[],[]]
Output: [2,4,5,6]
Explanation: The given graph is shown above.

Example 2:

Input: graph = [[1,2,3,4],[1,2],[3,4],[0,4],[]]
Output: [4]

__Constraints:__

n == graph.length
1 <= n <= 10^4
0 <= graph[i].length <= n
graph[i] is sorted in a strictly increasing order.
The graph may contain self-loops.
The number of edges in the graph will be in the range [1, 4 * 10^4].

__题目描述__:
在有向图中，以某个节点为起始节点，从该点出发，每一步沿着图中的一条有向边行走。如果到达的节点是终点（即它没有连出的有向边），则停止。

对于一个起始节点，如果从该节点出发，无论每一步选择沿哪条有向边行走，最后必然在有限步内到达终点，则将该起始节点称作是 安全 的。

返回一个由图中所有安全的起始节点组成的数组作为答案。答案数组中的元素应当按 升序 排列。

该有向图有 n 个节点，按 0 到 n - 1 编号，其中 n 是 graph 的节点数。图以下述形式给出：graph[i] 是编号 j 节点的一个列表，满足 (i, j) 是图的一条有向边。

__示例 :__

示例 1：

![有向图](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png)

输入：graph = [[1,2],[2,3],[5],[0],[5],[],[]]
输出：[2,4,5,6]
解释：示意图如上。

示例 2：

输入：graph = [[1,2,3,4],[1,2],[3,4],[0,4],[]]
输出：[4]

__提示:__

n == graph.length
1 <= n <= 10^4
0 <= graph[i].length <= n
graph[i] 按严格递增顺序排列。
图中可能包含自环。
图中边的数目在范围 [1, 4 * 10^4] 内。

__思路__:

拓扑排序
用一个标记数组记录遍历的状态
初始值设为 0 表示未访问
1 表示正在访问
2 表示访问完毕, 安全
3 表示在环内
使用 DFS 遍历图, 按照不同的标记加入结果
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) 
    {
        int n = graph.size();
        vector<int> result, tag(n);
        for (int i = 0; i < n; i++) if (dfs(graph, tag, i) == 2) result.emplace_back(i);
        return result;
    }
private:
    int dfs(vector<vector<int>>& graph, vector<int>& tag, int start) 
    {
        if (tag[start]) return tag[start] == 2 ? 2 : 3;
        tag[start] = 1;
        for (const auto i : graph[start]) if (dfs(graph, tag, i) == 3) return (tag[i] = 3);
        return (tag[start] = 2);
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
    def eventualSafeNodes(self, graph: List[List[int]]) -> List[int]:
        result, tag = [], [0] * (n := len(graph))
        
        def dfs(start: int) -> int:
            if tag[start] != 0:
                return 2 if tag[start] == 2 else 3
            tag[start] = 1
            for i in graph[start]:
                if dfs(i) == 3:
                    tag[i] = 3
                    return 3
            tag[start] = 2
            return 2
        
        for i in range(n):
            dfs(i) == 2 and result.append(i)
        return result
```
