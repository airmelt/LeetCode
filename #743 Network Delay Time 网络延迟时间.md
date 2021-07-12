# 743 Network Delay Time 网络延迟时间

__Description__:
You are given a network of n nodes, labeled from 1 to n. You are also given times, a list of travel times as directed edges times[i] = (ui, vi, wi), where ui is the source node, vi is the target node, and wi is the time it takes for a signal to travel from source to target.

We will send a signal from a given node k. Return the time it takes for all the n nodes to receive the signal. If it is impossible for all the n nodes to receive the signal, return -1.

__Example:__

Example 1:

[network](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

Input: times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
Output: 2

Example 2:

Input: times = [[1,2,1]], n = 2, k = 1
Output: 1

Example 3:

Input: times = [[1,2,1]], n = 2, k = 2
Output: -1

__Constraints:__

1 <= k <= n <= 100
1 <= times.length <= 6000
times[i].length == 3
1 <= ui, vi <= n
ui != vi
0 <= wi <= 100
All the pairs (ui, vi) are unique. (i.e., no multiple edges.)

__题目描述__:
有 n 个网络节点，标记为 1 到 n。

给你一个列表 times，表示信号经过 有向 边的传递时间。 times[i] = (ui, vi, wi)，其中 ui 是源节点，vi 是目标节点， wi 是一个信号从源节点传递到目标节点的时间。

现在，从某个节点 K 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 -1 。

__示例 :__

示例 1：

[网络](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

输入：times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
输出：2

示例 2：

输入：times = [[1,2,1]], n = 2, k = 1
输出：1

示例 3：

输入：times = [[1,2,1]], n = 2, k = 2
输出：-1

__提示:__

1 <= k <= n <= 100
1 <= times.length <= 6000
times[i].length == 3
1 <= ui, vi <= n
ui != vi
0 <= wi <= 100
所有 (ui, vi) 对都 互不相同（即，不含重复边）

__思路__:

disjkstra 算法
先构造出图
然后在图中更新距离
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) 
    {
        queue<int> q;
        vector<int> distance(n, INT_MAX);
        vector<vector<int>> graph(n, vector<int>(n, -1));
        for (const auto &time : times) graph[time[0] - 1][time[1] - 1] = time[2];
        q.push(k - 1);
        distance[k - 1] = 0;
        while(!q.empty())
        {
            int cur = q.front();
            q.pop();
            for (int i = 0; i < n; i++)
            {
                if (graph[cur][i] != -1 and distance[cur] + graph[cur][i] < distance[i])
                {
                    distance[i] = distance[cur] + graph[cur][i];
                    q.push(i);
                }
            }
        }
        return *max_element(distance.begin(), distance.end()) == INT_MAX ? -1 : *max_element(distance.begin(), distance.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        Queue<Integer> queue = new LinkedList<>();
        int graph[][] = new int[n][n], distance[] = new int[n], result = 0;
        Arrays.fill(distance, Integer.MAX_VALUE);
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) graph[i][j] = -1;
        for (int time[] : times) graph[time[0] - 1][time[1] - 1] = time[2];
        queue.offer(k - 1);
        distance[k - 1] = 0;
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            for (int i = 0; i < n; i++) {
                if (graph[cur][i] != -1 && distance[i] > distance[cur] + graph[cur][i]) {
                    distance[i] = distance[cur] + graph[cur][i];
                    queue.offer(i);
                }
            }
        }
        for (int i = 0; i < n; i++) result = Math.max(distance[i], result);
        return result == Integer.MAX_VALUE ? -1 : result;
    }
}
```

__Python__:

```Python
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        graph, q, distance = [[-1] * n for _ in range(n)], [k - 1], [float('inf')] * n
        distance[k - 1] = 0
        for time in times:
            graph[time[0] - 1][time[1] - 1] = time[2]
        while q:
            cur = q.pop(0)
            for i in range(n):
                if graph[cur][i] != -1 and distance[i] > distance[cur] + graph[cur][i]:
                    distance[i] = distance[cur] + graph[cur][i]
                    q.append(i)
        return max(distance) if max(distance) != float('inf') else -1
```
