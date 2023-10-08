# 1834 Single-Threaded CPU 单线程 CPU

__Description:__

You are given `n`​​​​​​ tasks labeled from `0` to `n - 1` represented by a 2D integer array `tasks`, where `tasks[i] = [enqueueTimei, processingTimei]` means that the `i ^ ​​​​​​th`​​​​ task will be available to process at `enqueueTimei` and will take `processingTimei` to finish processing.

You have a single-threaded CPU that can process at most one task at a time and will act in the following way:

- If the CPU is idle and there are no available tasks to process, the CPU remains idle.
- If the CPU is idle and there are available tasks, the CPU will choose the one with the __shortest processing time__. If multiple tasks have the same shortest processing time, it will choose the task with the smallest index.
- Once a task is started, the CPU will __process the entire task__ without stopping.
- The CPU can finish a task then start a new one instantly.

Return the order in which the CPU will process the tasks.

__Example:__

Example 1:

```text
Input: tasks = [[1,2],[2,4],[3,2],[4,1]]
Output: [0,2,3,1]
Explanation: The events go as follows: 
- At time = 1, task 0 is available to process. Available tasks = {0}.
- Also at time = 1, the idle CPU starts processing task 0. Available tasks = {}.
- At time = 2, task 1 is available to process. Available tasks = {1}.
- At time = 3, task 2 is available to process. Available tasks = {1, 2}.
- Also at time = 3, the CPU finishes task 0 and starts processing task 2 as it is the shortest. Available tasks = {1}.
- At time = 4, task 3 is available to process. Available tasks = {1, 3}.
- At time = 5, the CPU finishes task 2 and starts processing task 3 as it is the shortest. Available tasks = {1}.
- At time = 6, the CPU finishes task 3 and starts processing task 1. Available tasks = {}.
- At time = 10, the CPU finishes task 1 and becomes idle.
```

Example 2:

```text
Input: tasks = [[7,10],[7,12],[7,5],[7,4],[7,2]]
Output: [4,3,2,0,1]
Explanation: The events go as follows:
- At time = 7, all the tasks become available. Available tasks = {0,1,2,3,4}.
- Also at time = 7, the idle CPU starts processing task 4. Available tasks = {0,1,2,3}.
- At time = 9, the CPU finishes task 4 and starts processing task 3. Available tasks = {0,1,2}.
- At time = 13, the CPU finishes task 3 and starts processing task 2. Available tasks = {0,1}.
- At time = 18, the CPU finishes task 2 and starts processing task 0. Available tasks = {1}.
- At time = 28, the CPU finishes task 0 and starts processing task 1. Available tasks = {}.
- At time = 40, the CPU finishes task 1 and becomes idle.
```

__Constraints:__

- `tasks.length == n`
- `1 <= n <= 10 ^ 5`
- `1 <= enqueueTimei, processingTimei <= 10 ^ 9`

__题目描述:__

给你一个二维数组 `tasks` ，用于表示 `n`​​​​​​ 项从 `0` 到 `n - 1` 编号的任务。其中 `tasks[i] = [enqueueTimei, processingTimei]` 意味着第 `i ^ ​​​​​​`​​​​ 项任务将会于 `enqueueTimei` 时进入任务队列，需要 `processingTimei` 的时长完成执行。

现有一个单线程 CPU ，同一时间只能执行 最多一项 任务，该 CPU 将会按照下述方式运行：

- 如果 CPU 空闲，且任务队列中没有需要执行的任务，则 CPU 保持空闲状态。
- 如果 CPU 空闲，但任务队列中有需要执行的任务，则 CPU 将会选择 __执行时间最短__ 的任务开始执行。如果多个任务具有同样的最短执行时间，则选择下标最小的任务开始执行。
- 一旦某项任务开始执行，CPU 在 __执行完整个任务__ 前都不会停止。
- CPU 可以在完成一项任务后，立即开始执行一项新任务。

返回 CPU 处理任务的顺序。

__示例:__

示例 1：

```text
输入：tasks = [[1,2],[2,4],[3,2],[4,1]]
输出：[0,2,3,1]
解释：事件按下述流程运行： 
- time = 1 ，任务 0 进入任务队列，可执行任务项 = {0}
- 同样在 time = 1 ，空闲状态的 CPU 开始执行任务 0 ，可执行任务项 = {}
- time = 2 ，任务 1 进入任务队列，可执行任务项 = {1}
- time = 3 ，任务 2 进入任务队列，可执行任务项 = {1, 2}
- 同样在 time = 3 ，CPU 完成任务 0 并开始执行队列中用时最短的任务 2 ，可执行任务项 = {1}
- time = 4 ，任务 3 进入任务队列，可执行任务项 = {1, 3}
- time = 5 ，CPU 完成任务 2 并开始执行队列中用时最短的任务 3 ，可执行任务项 = {1}
- time = 6 ，CPU 完成任务 3 并开始执行任务 1 ，可执行任务项 = {}
- time = 10 ，CPU 完成任务 1 并进入空闲状态
```

示例 2：

```text
输入：tasks = [[7,10],[7,12],[7,5],[7,4],[7,2]]
输出：[4,3,2,0,1]
解释：事件按下述流程运行： 
- time = 7 ，所有任务同时进入任务队列，可执行任务项  = {0,1,2,3,4}
- 同样在 time = 7 ，空闲状态的 CPU 开始执行任务 4 ，可执行任务项 = {0,1,2,3}
- time = 9 ，CPU 完成任务 4 并开始执行任务 3 ，可执行任务项 = {0,1,2}
- time = 13 ，CPU 完成任务 3 并开始执行任务 2 ，可执行任务项 = {0,1}
- time = 18 ，CPU 完成任务 2 并开始执行任务 0 ，可执行任务项 = {1}
- time = 28 ，CPU 完成任务 0 并开始执行任务 1 ，可执行任务项 = {}
- time = 40 ，CPU 完成任务 1 并进入空闲状态
```

__提示：__

- `tasks.length == n`
- `1 <= n <= 10 ^ 5`
- `1 <= enqueueTimei, processingTimei <= 10 ^ 9`

__思路:__

```text
优先队列
将 tasks 的下标按照开始时间排序
如果优先队列为空, 直接跳到当前执行时间
将所有不大于当前时间的任务加到优先队列中
将当前时间加上执行时间
并将下标加入结果中
弹出当前优先队列的队首
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> getOrder(vector<vector<int>>& tasks) 
    {
        int n = tasks.size(), idx = 0;
        vector<int> orders(n), result;
        iota(orders.begin(), orders.end(), 0);
        sort(orders.begin(), orders.end(), [&](int i, int j) { return tasks[i].front() < tasks[j].front(); });
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        long long cur = 0;
        for (int i = 0; i < n; i++) 
        {
            if (pq.empty()) cur = max(cur, (long long)tasks[orders[idx]].front());
            while (idx < n and tasks[orders[idx]].front() <= cur) pq.emplace(tasks[orders[idx]][1], orders[idx++]);
            auto&& [process, r] = pq.top();
            cur += process;
            result.emplace_back(r);
            pq.pop();
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] getOrder(int[][] tasks) {
        int n = tasks.length, orderTasks[][] = new int[n][3], result[] = new int[n];
        for (int i = 0; i < n; i++) orderTasks[i] = new int[]{tasks[i][0], tasks[i][1], i};
        Arrays.sort(orderTasks, (a, b) -> a[0] - b[0]);
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] == b[1] ? a[2] - b[2] : a[1] - b[1]);
        for (int i = 0, cur = 0, idx = 0; idx < n;) {
            while (i < n && tasks[i][0] <= cur) pq.add(orderTasks[i++]);
            if (pq.isEmpty()) cur = tasks[i][0];
            else {
                int[] task = pq.poll();
                result[idx++] = task[2];
                cur += task[1];
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getOrder(self, tasks: List[List[int]]) -> List[int]:
        orders, queue, result, n, cur, idx = sorted(list(range(len(tasks))), key=lambda x: tasks[x][0]), [], deque(), len(tasks), 0, 0
            for i in range(n):
                if not queue:
                    cur = max(cur, tasks[orders[idx]][0])
                while idx < n and tasks[orders[idx]][0] <= cur:
                    heappush(queue, (tasks[idx][1], orders[idx]))
                    idx += 1
                process, r = heappop(queue)
                cur += process
                result.append(r)
        return list(result)
```
