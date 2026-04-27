# 2050 Parallel Courses III 并行课程 III

__Description:__

You are given an integer `n`, which indicates that there are `n` courses labeled from `1` to `n`. You are also given a 2D integer array `relations` where `relations[j] = [prevCoursej, nextCoursej]` denotes that course `prevCoursej` has to be completed __before__ course `nextCoursej` (prerequisite relationship). Furthermore, you are given a __0-indexed__ integer array `time` where `time[i]` denotes how many __months__ it takes to complete the `(i+1) ^ th` course.

You must find the minimum number of months needed to complete all the courses following these rules:

- You may start taking a course at __any time__ if the prerequisites are met.
- __Any number of courses__ can be taken at the __same time__.

Return the minimum number of months needed to complete all the courses.

Note: The test cases are generated such that it is possible to complete every course (i.e., the graph is a directed acyclic graph).

__Example:__

Example 1:

![2050-1](https://assets.leetcode.com/uploads/2021/10/07/ex1.png)

```text
Input: n = 3, relations = [[1,3],[2,3]], time = [3,2,5]
Output: 8
Explanation: The figure above represents the given graph and the time required to complete each course. 
We start course 1 and course 2 simultaneously at month 0.
Course 1 takes 3 months and course 2 takes 2 months to complete respectively.
Thus, the earliest time we can start course 3 is at month 3, and the total time required is 3 + 5 = 8 months.
```

Example 2:

![2050-2](https://assets.leetcode.com/uploads/2021/10/07/ex2.png)

```text
Input: n = 5, relations = [[1,5],[2,5],[3,5],[3,4],[4,5]], time = [1,2,3,4,5]
Output: 12
Explanation: The figure above represents the given graph and the time required to complete each course.
You can start courses 1, 2, and 3 at month 0.
You can complete them after 1, 2, and 3 months respectively.
Course 4 can be taken only after course 3 is completed, i.e., after 3 months. It is completed after 3 + 4 = 7 months.
Course 5 can be taken only after courses 1, 2, 3, and 4 have been completed, i.e., after max(1,2,3,7) = 7 months.
Thus, the minimum time needed to complete all the courses is 7 + 5 = 12 months.
```

__Constraints:__

- `1 <= n <= 5 * 10 ^ 4`
- `0 <= relations.length <= min(n * (n - 1) / 2, 5 * 10 ^ 4)`
- `relations[j].length == 2`
- `1 <= prevCoursej, nextCoursej <= n`
- `prevCoursej != nextCoursej`
- All the pairs `[prevCoursej, nextCoursej]` are __unique__.
- `time.length == n`
- `1 <= time[i] <= 10 ^ 4`
- The given graph is a directed acyclic graph.

__题目描述:__

给你一个整数 `n` ，表示有 `n` 节课，课程编号从 `1` 到 `n` 。同时给你一个二维整数数组 `relations` ，其中 `relations[j] = [prevCoursej, nextCoursej]` ，表示课程 `prevCoursej` 必须在课程 `nextCoursej` __之前__ 完成（先修课的关系）。同时给你一个下标从 __0__ 开始的整数数组 `time` ，其中 `time[i]` 表示完成第 `(i+1)` 门课程需要花费的 __月份__ 数。

请你根据以下规则算出完成所有课程所需要的 最少 月份数：

- 如果一门课的所有先修课都已经完成，你可以在 __任意__ 时间开始这门课程。
- 你可以 __同时__ 上 __任意门课程__ 。

请你返回完成所有课程所需要的 最少 月份数。

注意：测试数据保证一定可以完成所有课程（也就是先修课的关系构成一个有向无环图）。

__示例:__

示例 1:

![2050-3](https://assets.leetcode.com/uploads/2021/10/07/ex1.png)

```text
输入：n = 3, relations = [[1,3],[2,3]], time = [3,2,5]
输出：8
解释：上图展示了输入数据所表示的先修关系图，以及完成每门课程需要花费的时间。
你可以在月份 0 同时开始课程 1 和 2 。
课程 1 花费 3 个月，课程 2 花费 2 个月。
所以，最早开始课程 3 的时间是月份 3 ，完成所有课程所需时间为 3 + 5 = 8 个月。
```

示例 2：

![2050-4](https://assets.leetcode.com/uploads/2021/10/07/ex2.png)

```text
输入：n = 5, relations = [[1,5],[2,5],[3,5],[3,4],[4,5]], time = [1,2,3,4,5]
输出：12
解释：上图展示了输入数据所表示的先修关系图，以及完成每门课程需要花费的时间。
你可以在月份 0 同时开始课程 1 ，2 和 3 。
在月份 1，2 和 3 分别完成这三门课程。
课程 4 需在课程 3 之后开始，也就是 3 个月后。课程 4 在 3 + 4 = 7 月完成。
课程 5 需在课程 1，2，3 和 4 之后开始，也就是在 max(1,2,3,7) = 7 月开始。
所以完成所有课程所需的最少时间为 7 + 5 = 12 个月。
```

__提示：__

- `1 <= n <= 5 * 10 ^ 4`
- `0 <= relations.length <= min(n * (n - 1) / 2, 5 * 10 ^ 4)`
- `relations[j].length == 2`
- `1 <= prevCoursej, nextCoursej <= n`
- `prevCoursej != nextCoursej`
- 所有的先修课程对 `[prevCoursej, nextCoursej]` 都是 __互不相同__ 的。
- `time.length == n`
- `1 <= time[i] <= 10 ^ 4`
- 先修课程图是一个有向无环图。

__思路:__

```text
拓扑排序
设 dp[i] 表示完成第 i 门课程所需的最少时间
dp[i] = dp[i] + max(dp[j]) 其中 j 为 i 的先修课程
先将 relations 转换为邻接表 graph
计算每门课程的入度 degree
将入度为 0 的课程加入队列, 并更新 dp[i] 为 time[i]
当队列不为空时, 依次取出队首元素 u
遍历 u 的后继课程 v, 更新 dp[v] = max(dp[v], dp[u] + time[v])
当 v 的入度为 0 时, 将 v 加入队列, 并更新 dp[v] = dp[v] + time[v] 以及结果 result = max(result, dp[v])
时间复杂度为 O(N + M), 空间复杂度为 O(N + M), 其中 M 为边数, 即 relations 的长度
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    int minimumTime(int n, vector<vector<int>>& relations, vector<int>& time) 
    {
        vector<vector<int>> graph(n);
        vector<int> degree(n), dp(n);
        int result = 0;
        for (const auto& edge : relations)
        {
            graph[edge.front() - 1].emplace_back(edge.back() - 1);
            ++degree[edge.back() - 1];
        }
        queue<int> q;
        for (int i = 0; i < n; i++) 
        {
            if (!degree[i]) 
            {
                result = max(result, time[i]);
                dp[i] = time[i];
                q.push(i);
            }
        }
        while (!q.empty()) 
        {
            int u = q.front();
            q.pop();
            for (const auto& v : graph[u]) 
            {
                dp[v] = max(dp[u], dp[v]);
                --degree[v];
                if (!degree[v]) 
                {
                    q.push(v);
                    dp[v] += time[v];
                    result = max(result, dp[v]);
                }
            }
        }
        return result; 
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumTime(int n, int[][] relations, int[] time) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
        int degree[] = new int[n], dp[] = new int[n], result = 0;
        for (int[] edge : relations)
        {
            graph.get(edge[0] - 1).add(edge[1] - 1);
            ++degree[edge[1] - 1];
        }
        Queue<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            if (degree[i] == 0) {
                result = Math.max(result, time[i]);
                dp[i] = time[i];
                queue.add(i);
            }
        }
        while (!queue.isEmpty()) {
            int u = queue.poll();
            for (int v : graph.get(u)) {
                dp[v] = Math.max(dp[u], dp[v]);
                --degree[v];
                if (degree[v] == 0) {
                    queue.add(v);
                    dp[v] += time[v];
                    result = Math.max(result, dp[v]);
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumTime(self, n: int, relations: List[List[int]], time: List[int]) -> int:
        graph, degree, queue, dp, result = defaultdict(set), [0] * n, deque(), [0] * n, 0
        for u, v in relations:
            graph[u - 1].add(v - 1)
            degree[v - 1] += 1
        for i, v in enumerate(degree):
            if not v:
                queue.append(i)
                result, dp[i] = max(result, time[i]), time[i]
        while queue:
            u = queue.popleft()
            for v in graph[u]:
                dp[v] = max(dp[v], dp[u])
                degree[v] -= 1
                if not degree[v]:
                    dp[v] += time[v]
                    result = max(result, dp[v])
                    queue.append(v)
        return result
```
