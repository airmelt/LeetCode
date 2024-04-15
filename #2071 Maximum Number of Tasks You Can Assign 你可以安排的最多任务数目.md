# 2071 Maximum Number of Tasks You Can Assign 你可以安排的最多任务数目

__Description:__

You have `n` tasks and `m` workers. Each task has a strength requirement stored in a __0-indexed__ integer array `tasks`, with the `i ^ th` task requiring `tasks[i]` strength to complete. The strength of each worker is stored in a __0-indexed__ integer array `workers`, with the `j ^ th` worker having `workers[j]` strength. Each worker can only be assigned to a __single__ task and must have a strength __greater than or equal__ to the task's strength requirement (i.e., `workers[j] >= tasks[i]`).

Additionally, you have `pills` magical pills that will __increase a worker's strength__ by `strength`. You can decide which workers receive the magical pills, however, you may only give each worker __at most one__ magical pill.

Given the __0-indexed__ integer arrays `tasks` and `workers` and the integers `pills` and `strength`, return _the __maximum__ number of tasks that can be completed._

__Example:__

Example 1:

```text
Input: tasks = [3,2,1], workers = [0,3,3], pills = 1, strength = 1
Output: 3
Explanation:
We can assign the magical pill and tasks as follows:
```

- Give the magical pill to worker 0.
- Assign worker 0 to task 2 (0 + 1 >= 1)
- Assign worker 1 to task 1 (3 >= 2)
- Assign worker 2 to task 0 (3 >= 3)

Example 2:

```text
Input: tasks = [5,4], workers = [0,0,0], pills = 1, strength = 5
Output: 1
Explanation:
We can assign the magical pill and tasks as follows:
```

- Give the magical pill to worker 0.
- Assign worker 0 to task 0 (0 + 5 >= 5)

Example 3:

```text
Input: tasks = [10,15,30], workers = [0,10,10,10,10], pills = 3, strength = 10
Output: 2
Explanation:
We can assign the magical pills and tasks as follows:
```

- Give the magical pill to worker 0 and worker 1.
- Assign worker 0 to task 0 (0 + 10 >= 10)
- Assign worker 1 to task 1 (10 + 10 >= 15)

The last pill is not given because it will not make any worker strong enough for the last task.

__Constraints:__

- `n == tasks.length`
- `m == workers.length`
- `1 <= n, m <= 5 * 10 ^ 4`
- `0 <= pills <= m`
- `0 <= tasks[i], workers[j], strength <= 10 ^ 9`

__题目描述:__

给你 `n` 个任务和 `m` 个工人。每个任务需要一定的力量值才能完成，需要的力量值保存在下标从 __0__ 开始的整数数组 `tasks` 中，第 `i` 个任务需要 `tasks[i]` 的力量才能完成。每个工人的力量值保存在下标从 __0__ 开始的整数数组 `workers` 中，第 `j` 个工人的力量值为 `workers[j]` 。每个工人只能完成 __一个__ 任务，且力量值需要 __大于等于__ 该任务的力量要求值（即 `workers[j] >= tasks[i]` ）。

除此以外，你还有 `pills` 个神奇药丸，可以给 __一个工人的力量值__ 增加 `strength` 。你可以决定给哪些工人使用药丸，但每个工人 __最多__ 只能使用 __一片__ 药丸。

给你下标从 __0__ 开始的整数数组 `tasks` 和 `workers` 以及两个整数 `pills` 和 `strength` ，请你返回 __最多__ 有多少个任务可以被完成。

__示例:__

示例 1：

```text
输入：tasks = [3,2,1], workers = [0,3,3], pills = 1, strength = 1
输出：3
解释：
我们可以按照如下方案安排药丸：
```

- 给 0 号工人药丸。
- 0 号工人完成任务 2（0 + 1 >= 1）
- 1 号工人完成任务 1（3 >= 2）
- 2 号工人完成任务 0（3 >= 3）

示例 2：

```text
输入：tasks = [5,4], workers = [0,0,0], pills = 1, strength = 5
输出：1
解释：
我们可以按照如下方案安排药丸：
```

- 给 0 号工人药丸。
- 0 号工人完成任务 0（0 + 5 >= 5）

示例 3：

```text
输入：tasks = [10,15,30], workers = [0,10,10,10,10], pills = 3, strength = 10
输出：2
解释：
我们可以按照如下方案安排药丸：
```

- 给 0 号和 1 号工人药丸。
- 0 号工人完成任务 0（0 + 10 >= 10）
- 1 号工人完成任务 1（10 + 10 >= 15）

示例 4：

```text
输入：tasks = [5,9,8,5,9], workers = [1,6,4,2,6], pills = 1, strength = 5
输出：3
解释：
我们可以按照如下方案安排药丸：
```

- 给 2 号工人药丸。
- 1 号工人完成任务 0（6 >= 5）
- 2 号工人完成任务 2（4 + 5 >= 8）
- 4 号工人完成任务 3（6 >= 5）

__提示：__

- `n == tasks.length`
- `m == workers.length`
- `1 <= n, m <= 5 * 10 ^ 4`
- `0 <= pills <= m`
- `0 <= tasks[i], workers[j], strength <= 10 ^ 9`

__思路:__

```text
二分 ➕ 双端队列
将 tasks 和 workers 排序
使用二分查找找到一个值使得 mid 能够满足 check 函数, mid + 1 不能满足 check 函数
check 函数的作用是判断是否能够完成 mid 个任务
使用双端队列保存当前的工人
如果吃药的工人能够完成当前的任务, 就加入到队列中, 加入的工人最多为 mid 个
如果一个工人可以不吃药完成当前的任务, 就从队列中删除力气最大的工人
如果一个工人吃药能够完成当前的任务, 就从队列中删除力气最小的工人
由于遍历任务是从后往前遍历的, 即任务的力气要求是递减的, 所以可以保证队列中的工人一定可以完成当前的任务, 并且能够充分利用药
当没人能够完成当前的任务时或者药吃完了, 就返回 false
时间复杂度为 O(NlogN + MlogM), 空间复杂度为 O(logN + logM + M), 其中 N 为 tasks 的长度, M 为 workers 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxTaskAssign(vector<int>& tasks, vector<int>& workers, int pills, int strength) 
    {
        int m = tasks.size(), n = workers.size(), left = 1, right = min(m, n), result = 0, mid = 0;
        sort(tasks.begin(), tasks.end());
        sort(workers.begin(), workers.end());
        auto check = [&](int mid) -> bool 
        {
            deque<int> cur;
            for (int i = mid - 1, idx = n - 1, p = pills; i > -1; i--) 
            {
                while (idx >= n - mid and workers[idx] + strength >= tasks[i]) cur.push_back(workers[idx--]);
                if (!cur.empty() and cur.front() >= tasks[i]) cur.pop_front();
                else 
                {
                    if (!p or cur.empty()) return false;
                    --p;
                    cur.pop_back();
                }
            }
            return true;
        };
        while (left <= right) 
        {
            if (check(mid = left + ((right - left) >> 1))) 
            {
                result = mid;
                left = mid + 1;
            }
            else right = mid - 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int[] tasks, workers;
    private int pills, strength;

    public int maxTaskAssign(int[] tasks, int[] workers, int pills, int strength) {
        int m = tasks.length, n = workers.length, left = 1, right = Math.min(m, n), result = 0, mid = 0;
        Arrays.sort(tasks);
        Arrays.sort(workers);
        this.tasks = tasks;
        this.workers = workers;
        this.pills = pills;
        this.strength = strength;
        while (left <= right) {
            if (check(mid = left + ((right - left) >>> 1))) {
                result = mid;
                left = mid + 1;
            } else right = mid - 1;
        }
        return result;
    }

    private boolean check(int mid) {
        Deque<Integer> deque = new ArrayDeque<>();
        for (int i = mid - 1, n = workers.length, idx = n - 1, p = pills; i > -1; i--) {
            while (idx >= n - mid && workers[idx] + strength >= tasks[i]) deque.add(workers[idx--]);
            if (!deque.isEmpty() && deque.getFirst() >= tasks[i]) deque.removeFirst();
            else {
                if (p == 0 || deque.isEmpty()) return false;
                --p;
                deque.removeLast();
            }
        }
        return true;
    }
}
```

__Python__:

```Python
from sortedcontainers import SortedList
class Solution:
    def maxTaskAssign(self, tasks: List[int], workers: List[int], pills: int, strength: int) -> int:
        tasks.sort()
        workers.sort()
        def check(x: int) -> bool:
            p, ws = pills, SortedList(workers[-x:])
            for i in range(x - 1, -1, -1):
                if ws[-1] >= tasks[i]: 
                    ws.pop()
                elif not p or ws[-1] + strength < tasks[i]: 
                    return True
                else: 
                    p -= 1 
                    ws.pop(ws.bisect_left(tasks[i] - strength))
            return False
        return bisect_left(range(0, 1 + min(len(tasks), len(workers))), True, key = check) - 1
```
