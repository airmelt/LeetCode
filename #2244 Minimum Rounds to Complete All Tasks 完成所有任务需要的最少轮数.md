# 2244 Minimum Rounds to Complete All Tasks 完成所有任务需要的最少轮数

__Description:__

You are given a __0-indexed__ integer array `tasks`, where `tasks[i]` represents the difficulty level of a task. In each round, you can complete either 2 or 3 tasks of the __same difficulty level__.

Return _the __minimum__ rounds required to complete all the tasks, or_ `-1` _if it is not possible to complete all the tasks._

__Example:__

Example 1:

```text
Input: tasks = [2,2,3,3,2,4,4,4,4,4]
Output: 4
Explanation: To complete all the tasks, a possible plan is:
```

- In the first round, you complete 3 tasks of difficulty level 2.
- In the second round, you complete 2 tasks of difficulty level 3.
- In the third round, you complete 3 tasks of difficulty level 4.
- In the fourth round, you complete 2 tasks of difficulty level 4.  

It can be shown that all the tasks cannot be completed in fewer than 4 rounds, so the answer is 4.

Example 2:

```text
Input: tasks = [2,3,3]
Output: -1
Explanation: There is only 1 task of difficulty level 2, but in each round, you can only complete either 2 or 3 tasks of the same difficulty level. Hence, you cannot complete all the tasks, and the answer is -1.
```

__Constraints:__

- `1 <= tasks.length <= 10 ^ 5`
- `1 <= tasks[i] <= 10 ^ 9`

Note: This question is the same as 2870: Minimum Number of Operations to Make Array Empty.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `tasks` ，其中 `tasks[i]` 表示任务的难度级别。在每一轮中，你可以完成 2 个或者 3 个 __相同难度级别__ 的任务。

返回完成所有任务需要的 __最少__ 轮数，如果无法完成所有任务，返回 `-1` 。

__示例:__

示例 1：

```text
输入：tasks = [2,2,3,3,2,4,4,4,4,4]
输出：4
解释：要想完成所有任务，一个可能的计划是：
```

- 第一轮，完成难度级别为 2 的 3 个任务。
- 第二轮，完成难度级别为 3 的 2 个任务。
- 第三轮，完成难度级别为 4 的 3 个任务。
- 第四轮，完成难度级别为 4 的 2 个任务。

可以证明，无法在少于 4 轮的情况下完成所有任务，所以答案为 4 。

示例 2：

```text
输入：tasks = [2,3,3]
输出：-1
解释：难度级别为 2 的任务只有 1 个，但每一轮执行中，只能选择完成 2 个或者 3 个相同难度级别的任务。因此，无法完成所有任务，答案为 -1 。
```

__提示：__

- `1 <= tasks.length <= 10 ^ 5`
- `1 <= tasks[i] <= 10 ^ 9`

__思路:__

```text
模拟
用哈希表记录 task 出现的次数
如果有一个 task 的次数为 1, 则无法完成所有任务, 返回 -1
否则, 尽可能多的完成 task, 使得每次完成 3 个任务
共需要 (v + 2) / 3 轮
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumRounds(vector<int>& tasks) 
    {
        unordered_map<int, int> m;
        for (const auto& task : tasks) ++m[task];
        int result = 0;
        for (const auto& [_, v] : m) 
        {
            if (v == 1) return -1;
            result += (v + 2) / 3;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumRounds(int[] tasks) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int task : tasks) map.merge(task, 1, Integer::sum);
        int result = 0;
        for (int v : map.values()) {
            if (v == 1) return -1;
            result += (v + 2) / 3;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumRounds(self, tasks: List[int]) -> int:
        return -1 if 1 in (c := Counter(tasks)).values() else sum((v + 2) // 3 for v in c.values())
```
