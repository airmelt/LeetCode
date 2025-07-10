# 2589 Minimum Time to Complete All Tasks 完成所有任务的最少时间

__Description:__

There is a computer that can run an unlimited number of tasks __at the same time__. You are given a 2D integer array `tasks` where `tasks[i] = [start_i, end_i, duration_i]` indicates that the `i ^ th` task should run for a total of `duration_i` seconds (not necessarily continuous) within the __inclusive__ time range `[start_i, end_i]`.

You may turn on the computer only when it needs to run a task. You can also turn it off if it is idle.

Return the minimum time during which the computer should be turned on to complete all tasks.

__Example:__

Example 1:

```text
Input: tasks = [[2,3,1],[4,5,1],[1,5,2]]
Output: 2
Explanation: 
```

- The first task can be run in the inclusive time range [2, 2].
- The second task can be run in the inclusive time range [5, 5].
- The third task can be run in the two inclusive time ranges [2, 2] and [5, 5].

The computer will be on for a total of 2 seconds.

Example 2:

```text
Input: tasks = [[1,3,2],[2,5,3],[5,6,2]]
Output: 4
Explanation: 
```

- The first task can be run in the inclusive time range [2, 3].
- The second task can be run in the inclusive time ranges [2, 3] and [5, 5].
- The third task can be run in the two inclusive time range [5, 6].

The computer will be on for a total of 4 seconds.

__Constraints:__

- `1 <= tasks.length <= 2000`
- `tasks[i].length == 3`
- `1 <= start_i, end_i <= 2000`
- `1 <= duration_i <= end_i - start_i + 1`

__题目描述:__

你有一台电脑，它可以 __同时__ 运行无数个任务。给你一个二维整数数组 `tasks` ，其中 `tasks[i] = [start_i, end_i, duration_i]` 表示第 `i` 个任务需要在 __闭区间__ 时间段 `[start_i, end_i]` 内运行 `duration_i` 个整数时间点（但不需要连续）。

当电脑需要运行任务时，你可以打开电脑，如果空闲时，你可以将电脑关闭。

请你返回完成所有任务的情况下，电脑最少需要运行多少秒。

__示例:__

示例 1：

```text
输入：tasks = [[2,3,1],[4,5,1],[1,5,2]]
输出：2
解释：
```

- 第一个任务在闭区间 [2, 2] 运行。
- 第二个任务在闭区间 [5, 5] 运行。
- 第三个任务在闭区间 [2, 2] 和 [5, 5] 运行。

电脑总共运行 2 个整数时间点。

示例 2：

```text
输入：tasks = [[1,3,2],[2,5,3],[5,6,2]]
输出：4
解释：
```

- 第一个任务在闭区间 [2, 3] 运行
- 第二个任务在闭区间 [2, 3] 和 [5, 5] 运行。
- 第三个任务在闭区间 [5, 6] 运行。

电脑总共运行 4 个整数时间点。

__提示：__

- `1 <= tasks.length <= 2000`
- `tasks[i].length == 3`
- `1 <= start_i, end_i <= 2000`
- `1 <= duration_i <= end_i - start_i + 1`

__思路:__

```text
排序 ➕ 贪心
将任务按照结束时间排序
程序最多需要运行的时间点数目为 2000, 或者 tasks 中最后一个任务的结束时间
使用一个布尔数组 run 来记录每个时间点是否被运行过
遍历每个任务, 对于每个任务, 检查它的开始时间和结束时间之间的时间点
如果某个时间点已经被运行过, 则将任务的运行时间减去 1
如果任务的运行时间大于 0,
则从结束时间向前遍历, 找到未被运行的时间点, 将其标记为已运行, 并将任务的运行时间减去 1, 因为越晚运行越有可能和满足后面的任务
直到任务的运行时间为 0
最后返回 run 数组中被运行的时间点数
时间复杂度为 O(NlogN + MN), 空间复杂度为 O(M), M 为任务的结束时间
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findMinimumTime(vector<vector<int>>& tasks) 
    {
        sort(tasks.begin(), tasks.end(), [](auto& a, auto& b) { return a[1] < b[1]; });
        int result = 0, start = 0, end = 0, d = 0, i = 0;
        vector<bool> run(tasks.back()[1] + 1);
        for (const auto& task : tasks) 
        {
            for (start = task[0], end = task[1], d = task[2], i = start; i <= end; i++) if (run[i]) --d;
            if (d > 0) 
            {
                for (i = end; d > 0; i--) 
                {
                    if (!run[i]) 
                    {
                        run[i] = true;
                        --d;
                        ++result;
                    }
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
    public int findMinimumTime(int[][] tasks) {
        Arrays.sort(tasks, (a, b) -> a[1] - b[1]);
        int result = 0, start = 0, end = 0, d = 0, i = 0;
        var run = new boolean[tasks[tasks.length - 1][1] + 1];
        for (int[] task : tasks) {
            for (start = task[0], end = task[1], d = task[2], i = start; i <= end; i++) if (run[i]) --d;
            if (d > 0) {
                for (i = end; d > 0; i--) {
                    if (!run[i]) {
                        run[i] = true;
                        --d;
                        ++result;
                    }
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
    def findMinimumTime(self, tasks: List[List[int]]) -> int:
        tasks.sort(key=lambda t: t[1])
        run = [False] * (tasks[-1][1] + 1)
        for start, end, d in tasks:
            if (d := d - sum(run[start: end + 1])) > 0:
                for i in range(end, start - 1, -1):
                    if run[i]:
                        continue
                    run[i] = True
                    d -= 1
                    if not d:
                        break
        return sum(run)
```
