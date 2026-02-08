# 2895 Minimum Processing Time 最小处理时间

__Description:__

You have a certain number of processors, each having 4 cores. The number of tasks to be executed is four times the number of processors. Each task must be assigned to a unique core, and each core can only be used once.

You are given an array `processorTime` representing the time each processor becomes available and an array `tasks` representing how long each task takes to complete. Return the _minimum_ time needed to complete all tasks.

__Example:__

Example 1:

```text
Input: processorTime = [8,10], tasks = [2,2,3,1,8,7,4,5]

Output: 16

Explanation:

Assign the tasks at indices 4, 5, 6, 7 to the first processor which becomes available at time = 8, and the tasks at indices 0, 1, 2, 3 to the second processor which becomes available at time = 10.

The time taken by the first processor to finish the execution of all tasks is max(8 + 8, 8 + 7, 8 + 4, 8 + 5) = 16.

The time taken by the second processor to finish the execution of all tasks is max(10 + 2, 10 + 2, 10 + 3, 10 + 1) = 13.
```

Example 2:

```text
Input: processorTime = [10,20], tasks = [2,3,1,2,5,8,4,3]

Output: 23

Explanation:

Assign the tasks at indices 1, 4, 5, 6 to the first processor and the others to the second processor.

The time taken by the first processor to finish the execution of all tasks is max(10 + 3, 10 + 5, 10 + 8, 10 + 4) = 18.

The time taken by the second processor to finish the execution of all tasks is max(20 + 2, 20 + 1, 20 + 2, 20 + 3) = 23.
```

__Constraints:__

- `1 <= n == processorTime.length <= 25000`
- `1 <= tasks.length <= 10 ^ 5`
- `0 <= processorTime[i] <= 10 ^ 9`
- `1 <= tasks[i] <= 10 ^ 9`
- `tasks.length == 4 * n`

__题目描述:__

你有 `n` 颗处理器，每颗处理器都有 `4` 个核心。现有 `n * 4` 个待执行任务，每个核心只执行 __一次__ 任务。

给你一个下标从 __0__ 开始的整数数组 `processorTime` ，表示每颗处理器最早空闲时间。另给你一个下标从 __0__ 开始的整数数组 `tasks` ，表示执行每个任务所需的时间。返回所有任务都执行完毕需要的 __最小时间__ 。

注意：每个核心独立执行任务。

__示例:__

示例 1：

```text
输入：processorTime = [8,10], tasks = [2,2,3,1,8,7,4,5]
输出：16
解释：
最优的方案是将下标为 4, 5, 6, 7 的任务分配给第一颗处理器（最早空闲时间 time = 8），下标为 0, 1, 2, 3 的任务分配给第二颗处理器（最早空闲时间 time = 10）。 
第一颗处理器执行完所有任务需要花费的时间 = max(8 + 8, 8 + 7, 8 + 4, 8 + 5) = 16 。
第二颗处理器执行完所有任务需要花费的时间 = max(10 + 2, 10 + 2, 10 + 3, 10 + 1) = 13 。
因此，可以证明执行完所有任务需要花费的最小时间是 16 。
```

示例 2：

```text
输入：processorTime = [10,20], tasks = [2,3,1,2,5,8,4,3]
输出：23
解释：
最优的方案是将下标为 1, 4, 5, 6 的任务分配给第一颗处理器（最早空闲时间 time = 10），下标为 0, 2, 3, 7 的任务分配给第二颗处理器（最早空闲时间 time = 20）。 
第一颗处理器执行完所有任务需要花费的时间 = max(10 + 3, 10 + 5, 10 + 8, 10 + 4) = 18 。 
第二颗处理器执行完所有任务需要花费的时间 = max(20 + 2, 20 + 1, 20 + 2, 20 + 3) = 23 。 
因此，可以证明执行完所有任务需要花费的最小时间是 23 。
```

__提示：__

- `1 <= n == processorTime.length <= 25000`
- `1 <= tasks.length <= 10 ^ 5`
- `0 <= processorTime[i] <= 10 ^ 9`
- `1 <= tasks[i] <= 10 ^ 9`
- `tasks.length == 4 * n`

__思路:__

```text
贪心
将 processorTime 从小到大排序
将 tasks 从大到小排序
越小的 processor 执行越大的 task
将 task 按照每 4 个一组分给 processor
计算最大值即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要时间复杂度瓶颈在排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minProcessingTime(vector<int>& processorTime, vector<int>& tasks) 
    {
        sort(processorTime.begin(), processorTime.end());
        sort(tasks.begin(), tasks.end(), std::greater<>());
        int n = processorTime.size(), result = 0;
        for (int i = 0; i < n; i++) result = max(result, processorTime[i] + tasks[i << 2]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minProcessingTime(List<Integer> processorTime, List<Integer> tasks) {
        Collections.sort(processorTime);
        Collections.sort(tasks, Collections.reverseOrder());
        int n = processorTime.size(), result = 0;
        for (int i = 0; i < n; i++) result = Math.max(result, processorTime.get(i) + tasks.get(i << 2));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minProcessingTime(self, processorTime: List[int], tasks: List[int]) -> int:
        return max(t + p for t, p in zip(sorted(tasks, reverse=True)[::4], sorted(processorTime)))
```
