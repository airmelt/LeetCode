# 1882 Process Tasks Using Servers 使用服务器处理任务

__Description:__

You are given two __0-indexed__ integer arrays `servers` and `tasks` of lengths `n`​​​​​​ and `m`​​​​​​ respectively. `servers[i]` is the __weight__ of the `i ^ ​​​​​​th`​​​​ server, and `tasks[j]` is the __time needed__ to process the `j ^ ​​​​​​th`​​​​ task __in seconds__.

Tasks are assigned to the servers using a task queue. Initially, all servers are free, and the queue is empty.

At second `j`, the `j ^ th` task is __inserted__ into the queue (starting with the `0 ^ th` task being inserted at second `0`). As long as there are free servers and the queue is not empty, the task in the front of the queue will be assigned to a free server with the __smallest weight__, and in case of a tie, it is assigned to a free server with the __smallest index__.

If there are no free servers and the queue is not empty, we wait until a server becomes free and immediately assign the next task. If multiple servers become free at the same time, then multiple tasks from the queue will be assigned in order of insertion following the weight and index priorities above.

A server that is assigned task `j` at second `t` will be free again at second `t + tasks[j]`.

Build an array `ans`​​​​ of length `m`, where `ans[j]` is the __index__ of the server the `j ^ ​​​​​​th` task will be assigned to.

Return _the array_ `ans`​​​​.

__Example:__

Example 1:

```text
Input: servers = [3,3,2], tasks = [1,2,3,2,1,2]
Output: [2,2,0,2,1,2]
Explanation: Events in chronological order go as follows:
```

- At second 0, task 0 is added and processed using server 2 until second 1.
- At second 1, server 2 becomes free. Task 1 is added and processed using server 2 until second 3.
- At second 2, task 2 is added and processed using server 0 until second 5.
- At second 3, server 2 becomes free. Task 3 is added and processed using server 2 until second 5.
- At second 4, task 4 is added and processed using server 1 until second 5.
- At second 5, all servers become free. Task 5 is added and processed using server 2 until second 7.

Example 2:

```text
Input: servers = [5,1,4,3,2], tasks = [2,1,2,4,5,2,1]
Output: [1,4,1,4,1,3,2]
Explanation: Events in chronological order go as follows: 
```

- At second 0, task 0 is added and processed using server 1 until second 2.
- At second 1, task 1 is added and processed using server 4 until second 2.
- At second 2, servers 1 and 4 become free. Task 2 is added and processed using server 1 until second 4.
- At second 3, task 3 is added and processed using server 4 until second 7.
- At second 4, server 1 becomes free. Task 4 is added and processed using server 1 until second 9.
- At second 5, task 5 is added and processed using server 3 until second 7.
- At second 6, task 6 is added and processed using server 2 until second 7.

__Constraints:__

- `servers.length == n`
- `tasks.length == m`
- `1 <= n, m <= 2 * 10 ^ 5`
- `1 <= servers[i], tasks[j] <= 2 * 10 ^ 5`

__题目描述:__

给你两个 __下标从 0 开始__ 的整数数组 `servers` 和 `tasks` ，长度分别为 `n`​​​​​​ 和 `m`​​​​​​ 。 `servers[i]` 是第 `i ^ ​​​​​​`​​​​ 台服务器的 __权重__ ，而 `tasks[j]` 是处理第 `j ^ ​​​​​​` 项任务 __所需要的时间__（单位:秒）。

你正在运行一个仿真系统，在处理完所有任务后，该系统将会关闭。每台服务器只能同时处理一项任务。第 `0` 项任务在第 `0` 秒可以开始处理，相应地，第 `j` 项任务在第 `j` 秒可以开始处理。处理第 `j` 项任务时，你需要为它分配一台 __权重最小__ 的空闲服务器。如果存在多台相同权重的空闲服务器，请选择 __下标最小__ 的服务器。如果一台空闲服务器在第 `t` 秒分配到第 `j` 项任务，那么在 `t + tasks[j]` 时它将恢复空闲状态。

如果没有空闲服务器，则必须等待，直到出现一台空闲服务器，并 尽可能早 地处理剩余任务。 如果有多项任务等待分配，则按照 下标递增 的顺序完成分配。

如果同一时刻存在多台空闲服务器，可以同时将多项任务分别分配给它们。

构建长度为 `m` 的答案数组 `ans` ，其中 `ans[j]` 是第 `j` 项任务分配的服务器的下标。

返回答案数组 `ans`​​​​ 。

__示例:__

示例 1：

```text
输入：servers = [3,3,2], tasks = [1,2,3,2,1,2]
输出：[2,2,0,2,1,2]
解释：事件按时间顺序如下：
```

- 0 秒时，第 0 项任务加入到任务队列，使用第 2 台服务器处理到 1 秒。
- 1 秒时，第 2 台服务器空闲，第 1 项任务加入到任务队列，使用第 2 台服务器处理到 3 秒。
- 2 秒时，第 2 项任务加入到任务队列，使用第 0 台服务器处理到 5 秒。
- 3 秒时，第 2 台服务器空闲，第 3 项任务加入到任务队列，使用第 2 台服务器处理到 5 秒。
- 4 秒时，第 4 项任务加入到任务队列，使用第 1 台服务器处理到 5 秒。
- 5 秒时，所有服务器都空闲，第 5 项任务加入到任务队列，使用第 2 台服务器处理到 7 秒。

示例 2：

```text
输入：servers = [5,1,4,3,2], tasks = [2,1,2,4,5,2,1]
输出：[1,4,1,4,1,3,2]
解释：事件按时间顺序如下：
```

- 0 秒时，第 0 项任务加入到任务队列，使用第 1 台服务器处理到 2 秒。
- 1 秒时，第 1 项任务加入到任务队列，使用第 4 台服务器处理到 2 秒。
- 2 秒时，第 1 台和第 4 台服务器空闲，第 2 项任务加入到任务队列，使用第 1 台服务器处理到 4 秒。
- 3 秒时，第 3 项任务加入到任务队列，使用第 4 台服务器处理到 7 秒。
- 4 秒时，第 1 台服务器空闲，第 4 项任务加入到任务队列，使用第 1 台服务器处理到 9 秒。
- 5 秒时，第 5 项任务加入到任务队列，使用第 3 台服务器处理到 7 秒。
- 6 秒时，第 6 项任务加入到任务队列，使用第 2 台服务器处理到 7 秒。

__提示：__

- `servers.length == n`
- `tasks.length == m`
- `1 <= n, m <= 2 * 10 ^ 5`
- `1 <= servers[i], tasks[j] <= 2 * 10 ^ 5`

__思路:__

```text
优先队列
用两个优先队列分别存储空闲服务器 idle 和忙碌服务器 busy
初始化 idle 为 [servers[i], i] 的优先队列(小根堆), busy 为空
记录当前时间 cur = 0
每次将当前时间更新为 max(cur, i)
当有忙碌服务器时, 检查当前时间是否已经执行完任务, 如果执行完任务, 将对应服务器加入 idle, 并更新当前时间为忙碌服务器的完成时间
当没有空闲服务器时, 取出最早完成任务的服务器, 并将其加入 idle, 并更新当前时间为忙碌服务器的完成时间
执行完上述操作后, 从 idle 中取出一个服务器, 将其加入 busy, 记录其完成时间和服务器编号, 并将其加入结果数组
时间复杂度为 O(M + NlogM), 空间复杂度为 O(M), 其中 M 为服务器数量, N 为任务数量
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> assignTasks(vector<int>& servers, vector<int>& tasks) 
    {
        int m = servers.size(), n = tasks.size(), cur = 0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> busy, idle;
        for (int i = 0; i < m; i++) idle.emplace(servers[i], i);
        vector<int> result;
        for (int i = 0; i < n; i++) 
        {
            cur = max(cur, i);
            while ((!busy.empty() and busy.top().first <= cur) or idle.empty())
            {
                idle.emplace(servers[busy.top().second], busy.top().second);
                cur = busy.top().first;
                busy.pop();
            }
            auto&& [_, idx] = idle.top();
            result.emplace_back(idx);
            busy.emplace(cur + tasks[i], idx);
            idle.pop();
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] assignTasks(int[] servers, int[] tasks) {
        PriorityQueue<int[]> idle = new PriorityQueue<>((a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]), busy = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        for (int i = 0, m = servers.length; i < m; i++) idle.add(new int[]{servers[i], i});
        int cur = 0, n = tasks.length, result[] = new int[n];
        for (int i = 0; i < n; i++) {
            cur = Math.max(cur, i);
            while ((!busy.isEmpty() && busy.peek()[0] <= cur) || idle.isEmpty()) {
                int[] task = busy.poll();
                idle.add(new int[]{servers[task[1]], task[1]});
                cur = task[0];
            }
            int[] server = idle.poll();
            result[i] = server[1];
            busy.add(new int[]{cur + tasks[i], server[1]});
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def assignTasks(self, servers: List[int], tasks: List[int]) -> List[int]:
        busy, idle, cur, result = [], [(server, i) for i, server in enumerate(servers)], 0, []
        heapify(idle)
        for i, task in enumerate(tasks):
            cur = max(cur, i)
            while (busy and busy[0][0] <= cur) or not idle:
                cur, idx = heappop(busy)
                heappush(idle, (servers[idx], idx))
            _, idx = heappop(idle)
            result.append(idx)
            heappush(busy, (cur + task, idx))
        return result
```
