# 1665 Minimum Initial Energy to Finish Tasks 完成所有任务的最少初始能量

__Description:__

You are given an array `tasks` where `tasks[i] = [actuali, minimumi]`:

- `actuali` is the actual amount of energy you __spend to finish__ the `i ^ th` task.
- `minimumi` is the minimum amount of energy you __require to begin__ the `i ^ th` task.

For example, if the task is `[10, 12]` and your current energy is `11`, you cannot start this task. However, if your current energy is `13`, you can complete this task, and your energy will be `3` after finishing it.

You can finish the tasks in any order you like.

Return the minimum initial amount of energy you will need to finish all the tasks.

__Example:__

Example 1:

```text
Input: tasks = [[1,2],[2,4],[4,8]]
Output: 8
Explanation:
Starting with 8 energy, we finish the tasks in the following order:
    - 3rd task. Now energy = 8 - 4 = 4.
    - 2nd task. Now energy = 4 - 2 = 2.
    - 1st task. Now energy = 2 - 1 = 1.
Notice that even though we have leftover energy, starting with 7 energy does not work because we cannot do the 3rd task.
```

Example 2:

```text
Input: tasks = [[1,3],[2,4],[10,11],[10,12],[8,9]]
Output: 32
Explanation:
Starting with 32 energy, we finish the tasks in the following order:
    - 1st task. Now energy = 32 - 1 = 31.
    - 2nd task. Now energy = 31 - 2 = 29.
    - 3rd task. Now energy = 29 - 10 = 19.
    - 4th task. Now energy = 19 - 10 = 9.
    - 5th task. Now energy = 9 - 8 = 1.
```

Example 3:

```text
Input: tasks = [[1,7],[2,8],[3,9],[4,10],[5,11],[6,12]]
Output: 27
Explanation:
Starting with 27 energy, we finish the tasks in the following order:
    - 5th task. Now energy = 27 - 5 = 22.
    - 2nd task. Now energy = 22 - 2 = 20.
    - 3rd task. Now energy = 20 - 3 = 17.
    - 1st task. Now energy = 17 - 1 = 16.
    - 4th task. Now energy = 16 - 4 = 12.
    - 6th task. Now energy = 12 - 6 = 6.
```

__Constraints:__

- `1 <= tasks.length <= 10 ^ 5`
- `1 <= actual​i <= minimumi <= 10 ^ 4`

__题目描述:__

给你一个任务数组 `tasks` ，其中 `tasks[i] = [actuali, minimumi]` :

- `actuali` 是完成第 `i` 个任务 __需要耗费__ 的实际能量。
- `minimumi` 是开始第 `i` 个任务前需要达到的最低能量。

比方说，如果任务为 `[10, 12]` 且你当前的能量为 `11` ，那么你不能开始这个任务。如果你当前的能量为 `13` ，你可以完成这个任务，且完成它后剩余能量为 `3` 。

你可以按照 任意顺序 完成任务。

请你返回完成所有任务的 最少 初始能量。

__示例:__

示例 1：

```text
输入：tasks = [[1,2],[2,4],[4,8]]
输出：8
解释：
一开始有 8 能量，我们按照如下顺序完成任务：
    - 完成第 3 个任务，剩余能量为 8 - 4 = 4 。
    - 完成第 2 个任务，剩余能量为 4 - 2 = 2 。
    - 完成第 1 个任务，剩余能量为 2 - 1 = 1 。
注意到尽管我们有能量剩余，但是如果一开始只有 7 能量是不能完成所有任务的，因为我们无法开始第 3 个任务。
```

示例 2：

```text
输入：tasks = [[1,3],[2,4],[10,11],[10,12],[8,9]]
输出：32
解释：
一开始有 32 能量，我们按照如下顺序完成任务：
    - 完成第 1 个任务，剩余能量为 32 - 1 = 31 。
    - 完成第 2 个任务，剩余能量为 31 - 2 = 29 。
    - 完成第 3 个任务，剩余能量为 29 - 10 = 19 。
    - 完成第 4 个任务，剩余能量为 19 - 10 = 9 。
    - 完成第 5 个任务，剩余能量为 9 - 8 = 1 。
```

示例 3：

```text
输入：tasks = [[1,7],[2,8],[3,9],[4,10],[5,11],[6,12]]
输出：27
解释：
一开始有 27 能量，我们按照如下顺序完成任务：
    - 完成第 5 个任务，剩余能量为 27 - 5 = 22 。
    - 完成第 2 个任务，剩余能量为 22 - 2 = 20 。
    - 完成第 3 个任务，剩余能量为 20 - 3 = 17 。
    - 完成第 1 个任务，剩余能量为 17 - 1 = 16 。
    - 完成第 4 个任务，剩余能量为 16 - 4 = 12 。
    - 完成第 6 个任务，剩余能量为 12 - 6 = 6 。
```

__提示：__

- `1 <= tasks.length <= 10 ^ 5`
- `1 <= actual​i <= minimumi <= 10 ^ 4`

__思路:__

```text
贪心
设每个任务为 [start, end]
比较两个任务 start1, end1 和 start2, end2
如果先完成 1 再完成 2, result = max(end1, start1 + end2)
如果先完成 2 再完成 1, result = max(end2, start2 + end1)
只需要比较 max(end1, start1 + end2) 和 max(end2, start2 + end1) 的大小即可
由于 end1 < start2 + end1, 所以实际上只需要比较 start1 + end2 和 start2 + end1 的大小
即 start1 - end1 和 start2 - end2 的大小
将任务按照完成任务所需能量的差值从小到大排序
能量为所有任务开始前所需能量的最大值加上所有任务完成后能量的和
时间复杂度为 O(Nlog), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumEffort(vector<vector<int>>& tasks) 
    {
        sort(tasks.begin(), tasks.end(), [](const auto& a, const auto& b) -> bool { return a.front() - a.back() < b.front() - b.back(); });
        int result = 0, s = 0;
        for (const auto& task : tasks) 
        {
            result = max(result, s + task.back());
            s += task.front();
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumEffort(int[][] tasks) {
        Arrays.sort(tasks, (a, b) -> (a[0] - a[1]) - (b[0] - b[1]));
        int result = 0, s = 0;
        for (int[] task : tasks) {
            result = Math.max(result, s + task[1]);
            s += task[0];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumEffort(self, tasks: List[List[int]]) -> int:
        tasks.sort(key=lambda x: x[0] - x[1])
        result = s = 0
        for start, end in tasks:
            result = max(result, s + end)
            s += start
        return result
```
