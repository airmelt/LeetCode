# 2365 Task Scheduler II 任务调度器 II

__Description:__

You are given a __0-indexed__ array of positive integers `tasks`, representing tasks that need to be completed __in order__, where `tasks[i]` represents the __type__ of the `i ^ th` task.

You are also given a positive integer `space`, which represents the __minimum__ number of days that must pass __after__ the completion of a task before another task of the __same__ type can be performed.

Each day, until all tasks have been completed, you must either:

- Complete the next task from `tasks`, or
- Take a break.

Return the minimum number of days needed to complete all tasks.

__Example:__

Example 1:

```text
Input: tasks = [1,2,1,2,3,1], space = 3
Output: 9
Explanation:
One way to complete all tasks in 9 days is as follows:
Day 1: Complete the 0th task.
Day 2: Complete the 1st task.
Day 3: Take a break.
Day 4: Take a break.
Day 5: Complete the 2nd task.
Day 6: Complete the 3rd task.
Day 7: Take a break.
Day 8: Complete the 4th task.
Day 9: Complete the 5th task.
It can be shown that the tasks cannot be completed in less than 9 days.
```

Example 2:

```text
Input: tasks = [5,8,8,5], space = 2
Output: 6
Explanation:
One way to complete all tasks in 6 days is as follows:
Day 1: Complete the 0th task.
Day 2: Complete the 1st task.
Day 3: Take a break.
Day 4: Take a break.
Day 5: Complete the 2nd task.
Day 6: Complete the 3rd task.
It can be shown that the tasks cannot be completed in less than 6 days.
```

__Constraints:__

- `1 <= tasks.length <= 10 ^ 5`
- `1 <= tasks[i] <= 10 ^ 9`
- `1 <= space <= tasks.length`

__题目描述:__

给你一个下标从 __0__ 开始的正整数数组 `tasks` ，表示需要 __按顺序__ 完成的任务，其中 `tasks[i]` 表示第 `i` 件任务的 __类型__ 。

同时给你一个正整数 `space` ，表示一个任务完成 __后__ ，另一个 __相同__ 类型任务完成前需要间隔的 __最少__ 天数。

在所有任务完成前的每一天，你都必须进行以下两种操作中的一种：

- 完成 `tasks` 中的下一个任务
- 休息一天

请你返回完成所有任务所需的 最少 天数。

__示例:__

示例 1：

```text
输入：tasks = [1,2,1,2,3,1], space = 3
输出：9
解释：
9 天完成所有任务的一种方法是：
第 1 天：完成任务 0 。
第 2 天：完成任务 1 。
第 3 天：休息。
第 4 天：休息。
第 5 天：完成任务 2 。
第 6 天：完成任务 3 。
第 7 天：休息。
第 8 天：完成任务 4 。
第 9 天：完成任务 5 。
可以证明无法少于 9 天完成所有任务。
```

示例 2：

```text
输入：tasks = [5,8,8,5], space = 2
输出：6
解释：
6 天完成所有任务的一种方法是：
第 1 天：完成任务 0 。
第 2 天：完成任务 1 。
第 3 天：休息。
第 4 天：休息。
第 5 天：完成任务 2 。
第 6 天：完成任务 3 。
可以证明无法少于 6 天完成所有任务。
```

__提示：__

- `1 <= tasks.length <= 10 ^ 5`
- `1 <= tasks[i] <= 10 ^ 9`
- `1 <= space <= tasks.length`

__思路:__

```text
模拟
用哈希表记录上一次任务的完成时间
首先先加上一天的时间, 用于完成任务
如果之前任务已经出现过, 则更新完成时间为上一次任务完成时间 + 间隔时间 + 1 和当前时间的最大值
最后返回完成时间
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long taskSchedulerII(vector<int>& tasks, int space) 
    {
        unordered_map<int, long long> last;
        long long result = 0LL;
        for (const auto& t : tasks)
        {
            ++result;
            if (last.count(t)) result = max(result, last[t] + space + 1LL);
            last[t] = result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long taskSchedulerII(int[] tasks, int space) {
        var last = new HashMap<Integer, Long>();
        long result = 0L;
        for (int t : tasks) {
            ++result;
            if (last.containsKey(t)) result = Math.max(result, last.get(t) + space + 1L);
            last.put(t, result);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def taskSchedulerII(self, tasks: List[int], space: int) -> int:
        result, last = 0, {}
        for t in tasks:
            result += 1
            if t in last:
                result = max(result, last[t] + space + 1)
            last[t] = result
        return result
```
