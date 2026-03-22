# 1462 Course Schedule IV 课程表IV

__Description:__

There are a total of  `numCourses` courses you have to take, labeled from  `0` to  `numCourses - 1`. You are given an array  `prerequisites` where  `prerequisites[i] = [ai, bi]` indicates that you __must__ take course  `ai` first if you want to take course  `bi`.

- For example, the pair  `[0, 1]` indicates that you have to take course  `0` before you can take course  `1`.

Prerequisites can also be __indirect__. If course  `a` is a prerequisite of course  `b`, and course  `b` is a prerequisite of course  `c`, then course  `a` is a prerequisite of course  `c`.

You are also given an array  `queries` where  `queries[j] = [uj, vj]`. For the  `j ^ th` query, you should answer whether course  `uj` is a prerequisite of course  `vj` or not.

Return _a boolean array_ `answer`_, where_ `answer[j]`_is the answer to the_ `j ^ th`_query._

__Example:__

Example 1:

![1462-1](https://assets.leetcode.com/uploads/2021/05/01/courses4-1-graph.jpg)

```text
Input: numCourses = 2, prerequisites = [[1,0]], queries = [[0,1],[1,0]]
Output: [false,true]
Explanation: The pair [1, 0] indicates that you have to take course 1 before you can take course 0.
Course 0 is not a prerequisite of course 1, but the opposite is true.
```

Example 2:

```text
Input: numCourses = 2, prerequisites = [], queries = [[1,0],[0,1]]
Output: [false,false]
Explanation: There are no prerequisites, and each course is independent.
```

Example 3:

![1462-2](https://assets.leetcode.com/uploads/2021/05/01/courses4-3-graph.jpg)

```text
Input: numCourses = 3, prerequisites = [[1,2],[1,0],[2,0]], queries = [[1,0],[1,2]]
Output: [true,true]
```

__Constraints:__

- `2 <= numCourses <= 100`
- `0 <= prerequisites.length <= (numCourses * (numCourses - 1) / 2)`
- `prerequisites[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- All the pairs  `[ai, bi]` are __unique__.
- The prerequisites graph has no cycles.
- `1 <= queries.length <= 10 ^ 4`
- `0 <= ui, vi <= n - 1`
- `ui != vi`

__题目描述:__

你总共需要上  `numCourses` 门课，课程编号依次为  `0` 到  `numCourses-1` 。你会得到一个数组  `prerequisite` ，其中  `prerequisites[i] = [ai, bi]` 表示如果你想选  `bi` 课程，你 __必须__ 先选  `ai` 课程。

- 有的课会有直接的先修课程，比如如果想上课程  `1` ，你必须先上课程  `0` ，那么会以  `[0,1]` 数对的形式给出先修课程数对。

先决条件也可以是 __间接__ 的。如果课程  `a` 是课程  `b` 的先决条件，课程  `b` 是课程  `c` 的先决条件，那么课程  `a` 就是课程  `c` 的先决条件。

你也得到一个数组  `queries` ，其中  `queries[j] = [uj, vj]`。对于第  `j` 个查询，您应该回答课程  `uj` 是否是课程  `vj` 的先决条件。

返回一个布尔数组  `answer` ，其中  `answer[j]` 是第  `j` 个查询的答案。

__示例:__

示例 1：

![1462-3](https://assets.leetcode.com/uploads/2021/05/01/courses4-1-graph.jpg)

```text
输入：numCourses = 2, prerequisites = [[1,0]], queries = [[0,1],[1,0]]
输出：[false,true]
解释：课程 0 不是课程 1 的先修课程，但课程 1 是课程 0 的先修课程。
```

示例 2：

```text
输入：numCourses = 2, prerequisites = [], queries = [[1,0],[0,1]]
输出：[false,false]
解释：没有先修课程对，所以每门课程之间是独立的。
```

示例 3：

![1462-4](https://assets.leetcode.com/uploads/2021/05/01/courses4-3-graph.jpg)

```text
输入：numCourses = 3, prerequisites = [[1,2],[1,0],[2,0]], queries = [[1,0],[1,2]]
输出：[true,true]
```

__提示：__

- `2 <= numCourses <= 100`
- `0 <= prerequisites.length <= (numCourses * (numCourses - 1) / 2)`
- `prerequisites[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- 每一对  `[ai, bi]` 都 __不同__
- 先修课程图中没有环。
- `0 <= ui, vi <= n - 1`
- `ui != vi`

__思路:__

```text
Floyd 算法
Floyd 算法可以用来判断连通图结点之间的正权重的最小值
在 [1, k] 中判断是否有 (i, j) 的最短路
如果有 dis[i][k] + dis[k][j] < dis[i][j]
说明以 k 为中间结点能减少总权重那么将 dis[i][j] 更新为 dis[i][k] + dis[k][j]
本题中只需要连通性即可
时间复杂度为 O(N ^ 3), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<bool> checkIfPrerequisite(int numCourses, vector<vector<int>>& prerequisites, vector<vector<int>>& queries) 
    {
        vector<vector<bool>> graph(numCourses, vector<bool>(numCourses));
        for (const auto& pre: prerequisites) graph[pre.front()][pre.back()] = true;
        for (int k = 0; k < numCourses; k++) for (int i = 0; i < numCourses; i++) for (int j = 0; j < numCourses; j++) if (graph[i][k] and graph[k][j]) graph[i][j] = true;
        vector<bool> result;
        for (const auto& q: queries) result.emplace_back(graph[q.front()][q.back()]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Boolean> checkIfPrerequisite(int numCourses, int[][] prerequisites, int[][] queries) {
        boolean[][] graph = new boolean[numCourses][numCourses];
        for (int[] pre: prerequisites) graph[pre[0]][pre[1]] = true;
        for (int k = 0; k < numCourses; k++) for (int i = 0; i < numCourses; i++) for (int j = 0; j < numCourses; j++) if (graph[i][k] && graph[k][j]) graph[i][j] = true;
        List<Boolean> result = new ArrayList<>();
        for (int[] q: queries) result.add(graph[q[0]][q[1]]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def checkIfPrerequisite(self, numCourses: int, prerequisites: List[List[int]], queries: List[List[int]]) -> List[bool]:
        graph = [[False] * numCourses for _ in range(numCourses)]
        for u, v in prerequisites:
            graph[u][v] = True
        for k in range(numCourses):
            for i in range(numCourses):
                for j in range(numCourses):
                    if graph[i][k] and graph[k][j]:
                        graph[i][j] = True
        return [graph[q[0]][q[1]] for q in queries]
```
