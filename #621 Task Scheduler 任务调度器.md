# 621 Task Scheduler 任务调度器

__Description__:
Given a characters array tasks, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.

However, there is a non-negative integer n that represents the cooldown period between two same tasks (the same letter in the array), that is that there must be at least n units of time between any two same tasks.

Return the least number of units of times that the CPU will take to finish all the given tasks.

__Example:__

Example 1:

Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation:
A -> B -> idle -> A -> B -> idle -> A -> B
There is at least 2 units of time between any two same tasks.

Example 2:

Input: tasks = ["A","A","A","B","B","B"], n = 0
Output: 6
Explanation: On this case any permutation of size 6 would work since n = 0.
["A","A","A","B","B","B"]
["A","B","A","B","A","B"]
["B","B","B","A","A","A"]
...
And so on.

Example 3:

Input: tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2
Output: 16
Explanation:
One possible solution is
A -> B -> C -> A -> D -> E -> A -> F -> G -> A -> idle -> idle -> A -> idle -> idle -> A

__Constraints:__

1 <= task.length <= 10^4
tasks[i] is upper-case English letter.
The integer n is in the range [0, 100].

__题目描述__:
给你一个用字符数组 tasks 表示的 CPU 需要执行的任务列表。其中每个字母表示一种不同种类的任务。任务可以以任意顺序执行，并且每个任务都可以在 1 个单位时间内执行完。在任何一个单位时间，CPU 可以完成一个任务，或者处于待命状态。

然而，两个 相同种类 的任务之间必须有长度为整数 n 的冷却时间，因此至少有连续 n 个单位时间内 CPU 在执行不同的任务，或者在待命状态。

你需要计算完成所有任务所需要的 最短时间 。

__示例 :__

示例 1：

输入：tasks = ["A","A","A","B","B","B"], n = 2
输出：8
解释：A -> B -> (待命) -> A -> B -> (待命) -> A -> B
     在本示例中，两个相同类型任务之间必须间隔长度为 n = 2 的冷却时间，而执行一个任务只需要一个单位时间，所以中间出现了（待命）状态。

示例 2：

输入：tasks = ["A","A","A","B","B","B"], n = 0
输出：6
解释：在这种情况下，任何大小为 6 的排列都可以满足要求，因为 n = 0
["A","A","A","B","B","B"]
["A","B","A","B","A","B"]
["B","B","B","A","A","A"]
...
诸如此类

示例 3：

输入：tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2
输出：16
解释：一种可能的解决方案是：
     A -> B -> C -> A -> D -> E -> A -> F -> G -> A -> (待命) -> (待命) -> A -> (待命) -> (待命) -> A

__提示:__

1 <= task.length <= 10^4
tasks[i] 是大写英文字母
n 的取值范围为 [0, 100]

__思路__:

统计任务的数量之后排序
选择数量最大的任务
比如 AAAABB, n = 2
A -> B -> A -> B -> A -> null -> A
这样至少需要 result = (count[25] - 1) * (n + 1) + 1 次
遍历 count 数组, 每次遇到与最大频率相等的, result 加 1
比如 AAABBB, n = 2
A -> B -> A -> B -> A -> B
返回 max(result, tasks.size()), 因为有可能 tasks 中的种类很多不够排
时间复杂度 O(n), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int leastInterval(vector<char>& tasks, int n) 
    {
        vector<int> count(26, 0);
        for (auto &task : tasks) ++count[task - 'A'];
        sort(count.begin(), count.end());
        int result = (count.back() - 1) * (n + 1) + 1, m = tasks.size();
        for (int i = 24; i > -1; i--) 
        {
            if (count[i] != count.back()) break;
            ++result;
        }
        return max(result, m);
    }
};
```

__Java__:

```Java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        int count[] = new int[26];
        for (char task : tasks) ++count[task - 'A'];
        Arrays.sort(count);
        int result = (count[25] - 1) * (n + 1) + 1, m = tasks.length;
        for (int i = 24; i > -1; i--) 
        {
            if (count[i] != count[25]) break;
            ++result;
        }
        return Math.max(result, m);
    }
}
```

__Python__:

```Python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        c = Counter(tasks)
        count = c.most_common()[0]
        del c[count[0]]
        result = (count[1] - 1) * (n + 1) + 1
        while c and c.most_common()[0][1] == count[1]:
            result += 1
            del c[c.most_common()[0][0]]
        return max(result, len(tasks))
```
