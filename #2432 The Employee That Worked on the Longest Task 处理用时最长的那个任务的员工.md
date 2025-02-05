# 2432 The Employee That Worked on the Longest Task 处理用时最长的那个任务的员工

__Description:__

There are `n` employees, each with a unique id from `0` to `n - 1`.

You are given a 2D integer array `logs` where `logs[i] = [id_i, leaveTime_i]` where:

- `id_i` is the id of the employee that worked on the `i ^ th` task, and
- `leaveTime_i` is the time at which the employee finished the `i ^ th` task. All the values `leaveTime_i` are __unique__.

Note that the `i ^ th` task starts the moment right after the `(i - 1) ^ th` task ends, and the `0 ^ th` task starts at time `0`.

Return the id of the employee that worked the task with the longest time. If there is a tie between two or more employees, return the smallest id among them.

__Example:__

Example 1:

```text
Input: n = 10, logs = [[0,3],[2,5],[0,9],[1,15]]
Output: 1
Explanation: 
Task 0 started at 0 and ended at 3 with 3 units of times.
Task 1 started at 3 and ended at 5 with 2 units of times.
Task 2 started at 5 and ended at 9 with 4 units of times.
Task 3 started at 9 and ended at 15 with 6 units of times.
The task with the longest time is task 3 and the employee with id 1 is the one that worked on it, so we return 1.
```

Example 2:

```text
Input: n = 26, logs = [[1,1],[3,7],[2,12],[7,17]]
Output: 3
Explanation: 
Task 0 started at 0 and ended at 1 with 1 unit of times.
Task 1 started at 1 and ended at 7 with 6 units of times.
Task 2 started at 7 and ended at 12 with 5 units of times.
Task 3 started at 12 and ended at 17 with 5 units of times.
The tasks with the longest time is task 1. The employee that worked on it is 3, so we return 3.
```

Example 3:

```text
Input: n = 2, logs = [[0,10],[1,20]]
Output: 0
Explanation: 
Task 0 started at 0 and ended at 10 with 10 units of times.
Task 1 started at 10 and ended at 20 with 10 units of times.
The tasks with the longest time are tasks 0 and 1. The employees that worked on them are 0 and 1, so we return the smallest id 0.
```

__Constraints:__

- `2 <= n <= 500`
- `1 <= logs.length <= 500`
- `logs[i].length == 2`
- `0 <= id_i <= n - 1`
- `1 <= leaveTime_i <= 500`
- `id_i != id_i+1`
- `leaveTime_i` are sorted in a strictly increasing order.

__题目描述:__

共有 `n` 位员工，每位员工都有一个从 `0` 到 `n - 1` 的唯一 id 。

给你一个二维整数数组 `logs` ，其中 `logs[i] = [id_i, leaveTime_i]` :

- `id_i` 是处理第 `i` 个任务的员工的 id ，且
- `leaveTime_i` 是员工完成第 `i` 个任务的时刻。所有 `leaveTime_i` 的值都是 __唯一__ 的。

注意，第 `i` 个任务在第 `(i - 1)` 个任务结束后立即开始，且第 `0` 个任务从时刻 `0` 开始。

返回处理用时最长的那个任务的员工的 id 。如果存在两个或多个员工同时满足，则返回几人中 最小 的 id 。

__示例:__

示例 1：

```text
输入：n = 10, logs = [[0,3],[2,5],[0,9],[1,15]]
输出：1
解释：
任务 0 于时刻 0 开始，且在时刻 3 结束，共计 3 个单位时间。
任务 1 于时刻 3 开始，且在时刻 5 结束，共计 2 个单位时间。
任务 2 于时刻 5 开始，且在时刻 9 结束，共计 4 个单位时间。
任务 3 于时刻 9 开始，且在时刻 15 结束，共计 6 个单位时间。
时间最长的任务是任务 3 ，而 id 为 1 的员工是处理此任务的员工，所以返回 1 。
```

示例 2：

```text
输入：n = 26, logs = [[1,1],[3,7],[2,12],[7,17]]
输出：3
解释：
任务 0 于时刻 0 开始，且在时刻 1 结束，共计 1 个单位时间。
任务 1 于时刻 1 开始，且在时刻 7 结束，共计 6 个单位时间。
任务 2 于时刻 7 开始，且在时刻 12 结束，共计 5 个单位时间。
任务 3 于时刻 12 开始，且在时刻 17 结束，共计 5 个单位时间。
时间最长的任务是任务 1 ，而 id 为 3 的员工是处理此任务的员工，所以返回 3 。
```

示例 3：

```text
输入：n = 2, logs = [[0,10],[1,20]]
输出：0
解释：
任务 0 于时刻 0 开始，且在时刻 10 结束，共计 10 个单位时间。
任务 1 于时刻 10 开始，且在时刻 20 结束，共计 10 个单位时间。
时间最长的任务是任务 0 和 1 ，处理这两个任务的员工的 id 分别是 0 和 1 ，所以返回最小的 0 。
```

__提示：__

- `2 <= n <= 500`
- `1 <= logs.length <= 500`
- `logs[i].length == 2`
- `0 <= id_i <= n - 1`
- `1 <= leaveTime_i <= 500`
- `id_i != id_i + 1`
- `leaveTime_i` 按严格递增顺序排列

__思路:__

```text
1. 排序
按照工作时长以及员工 id 排序
遍历 logs, 计算每个员工的工作时长
选择工作时长最长且编号最小的员工
时间复杂度为 O(MlogM), 空间复杂度为 O(logM), 其中 M 为 logs 的长度
2. 模拟
遍历 logs, 计算每个员工的工作时长
工作时长为 logs[i].back() - logs[i - 1].back()
第 0 个工作时长为 logs.front().back()
选择工作时长最长且编号最小的员工
时间复杂度为 O(M), 空间复杂度为 O(1), 其中 M 为 logs 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int hardestWorker(int n, vector<vector<int>>& logs) 
    {
        int result = logs.front().front(), cur = logs.front().back(), m = logs.size(), t = -1;
        for (int i = 1; i < m; i++) 
        {
            if ((t = logs[i].back() - logs[i - 1].back()) > cur || t == cur && logs[i].front() < result) 
            {
                result = logs[i].front();
                cur = t;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int hardestWorker(int n, int[][] logs) {
        int result = logs[0][0], cur = logs[0][1], m = logs.length, t = -1;
        for (int i = 1; i < m; i++) {
            if ((t = logs[i][1] - logs[i - 1][1]) > cur || t == cur && logs[i][0] < result) {
                result = logs[i][0];
                cur = t;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def hardestWorker(self, n: int, logs: List[List[int]]) -> int:
        return sorted([(logs[i][0], logs[i][1] - logs[i - 1][1] if i > 0 else logs[i][1]) for i in range(len(logs))], key=lambda item: (-item[1], item[0]))[0][0]
```
