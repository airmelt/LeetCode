# 1986 Minimum Number of Work Sessions to Finish the Tasks 完成任务的最少工作时间段

__Description:__

There are `n` tasks assigned to you. The task times are represented as an integer array `tasks` of length `n`, where the `i ^ th` task takes `tasks[i]` hours to finish. A __work session__ is when you work for __at most__ `sessionTime` consecutive hours and then take a break.

You should finish the given tasks in a way that satisfies the following conditions:

- If you start a task in a work session, you must complete it in the __same__ work session.
- You can start a new task __immediately__ after finishing the previous one.
- You may complete the tasks in __any order__.

Given `tasks` and `sessionTime`, return _the __minimum__ number of __work sessions__ needed to finish all the tasks following the conditions above._

The tests are generated such that `sessionTime` is __greater__ than or __equal__ to the __maximum__ element in `tasks[i]`.

__Example:__

Example 1:

```text
Input: tasks = [1,2,3], sessionTime = 3
Output: 2
Explanation: You can finish the tasks in two work sessions.
```

- First work session: finish the first and the second tasks in 1 + 2 = 3 hours.
- Second work session: finish the third task in 3 hours.

Example 2:

```text
Input: tasks = [3,1,3,1,1], sessionTime = 8
Output: 2
Explanation: You can finish the tasks in two work sessions.
```

- First work session: finish all the tasks except the last one in 3 + 1 + 3 + 1 = 8 hours.
- Second work session: finish the last task in 1 hour.

Example 3:

```text
Input: tasks = [1,2,3,4,5], sessionTime = 15
Output: 1
Explanation: You can finish all the tasks in one work session.
```

__Constraints:__

- `n == tasks.length`
- `1 <= n <= 14`
- `1 <= tasks[i] <= 10`
- `max(tasks[i]) <= sessionTime <= 15`

__题目描述:__

你被安排了 `n` 个任务。任务需要花费的时间用长度为 `n` 的整数数组 `tasks` 表示，第 `i` 个任务需要花费 `tasks[i]` 小时完成。一个 __工作时间段__ 中，你可以 __至多__ 连续工作 `sessionTime` 个小时，然后休息一会儿。

你需要按照如下条件完成给定任务：

- 如果你在某一个时间段开始一个任务，你需要在 __同一个__ 时间段完成它。
- 完成一个任务后，你可以 __立马__ 开始一个新的任务。
- 你可以按 __任意顺序__ 完成任务。

给你 `tasks` 和 `sessionTime` ，请你按照上述要求，返回完成所有任务所需要的 __最少__ 数目的 __工作时间段__ 。

测试数据保证 `sessionTime` __大于等于__ `tasks[i]` 中的 __最大值__ 。

__示例:__

示例 1：

```text
输入：tasks = [1,2,3], sessionTime = 3
输出：2
解释：你可以在两个工作时间段内完成所有任务。
```

- 第一个工作时间段：完成第一和第二个任务，花费 1 + 2 = 3 小时。
- 第二个工作时间段：完成第三个任务，花费 3 小时。
示例 2：

```text
输入：tasks = [3,1,3,1,1], sessionTime = 8
输出：2
解释：你可以在两个工作时间段内完成所有任务。
```

- 第一个工作时间段：完成除了最后一个任务以外的所有任务，花费 3 + 1 + 3 + 1 = 8 小时。
- 第二个工作时间段，完成最后一个任务，花费 1 小时。

示例 3：

```text
输入：tasks = [1,2,3,4,5], sessionTime = 15
输出：1
解释：你可以在一个工作时间段以内完成所有任务。
```

__提示：__

- `n == tasks.length`
- `1 <= n <= 14`
- `1 <= tasks[i] <= 10`
- `max(tasks[i]) <= sessionTime <= 15`

__思路:__

```text
状态压缩 ➕ 动态规划
使用二进制状态压缩，该位为 1 则执行了 task[i]
dp[i] 表示完成状态为 i 的任务所需要的最少工作时间段
转移方程为 dp[i] = min(dp[i], dp[i ^ sub] + 1), 其中 sub 是 i 的子集
快速计算子集可以用 sub = mask & (sub - 1)
预处理所有子集的和 sums[sub] = sum(tasks[i] for i in sub)
时间复杂度为 O(2 ^ N), 空间复杂度为 O(2 ^ N), 预处理时间复杂度为 O(2 ^ N), 计算子集时间复杂度为 O(2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSessions(vector<int> &tasks, int sessionTime) 
    {
        int n = tasks.size(), m = 1 << n;
        vector<int> dp(m, n), sums(m);
        for (int i = 0; i < n; i++) for (int j = 0; j < m; j++) if (j & (1 << i)) sums[j] += tasks[i];
        dp.front() = 0;
        for (int mask = 0; mask < m; mask++) for (int sub = mask; sub; sub = mask & (sub - 1)) if (sums[sub] <= sessionTime) dp[mask] = min(dp[mask], dp[mask ^ sub] + 1);
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minSessions(int[] tasks, int sessionTime) {
        int n = tasks.length, m = 1 << n, dp[] = new int[m], sums[] = new int[m];
        for (int i = 0; i < n; i++) for (int j = 0; j < m; j++) if ((j & (1 << i)) != 0) sums[j] += tasks[i];
        Arrays.fill(dp, n);
        dp[0] = 0;
        for (int mask = 0; mask < m; mask++) for (int sub = mask; sub != 0; sub = mask & (sub - 1)) if (sums[sub] <= sessionTime) dp[mask] = Math.min(dp[mask], dp[mask ^ sub] + 1);
        return dp[m - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def minSessions(self, tasks: List[int], sessionTime: int) -> int:
        total, sums = (m := 1 << (n := len(tasks))), defaultdict(int)
        for i in range(n):
            for j in range(m):
                if j & (1 << i):
                    sums[j] += tasks[i]
        dp = [0] + [n] * (m - 1)
        for mask in range(m):
            sub = mask
            while sub:
                if sums[sub] <= sessionTime:
                    dp[mask] = min(dp[mask], dp[mask ^ sub] + 1)
                sub = mask & (sub - 1)
        return dp[-1]
```
