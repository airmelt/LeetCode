# 1953 Maximum Number of Weeks for Which You Can Work 你可以工作的最大周数

__Description:__

There are `n` projects numbered from `0` to `n - 1`. You are given an integer array `milestones` where each `milestones[i]` denotes the number of milestones the `i ^ th` project has.

You can work on the projects following these two rules:

- Every week, you will finish __exactly one__ milestone of __one__ project. You __must__ work every week.
- You __cannot__ work on two milestones from the same project for two __consecutive__ weeks.

Once all the milestones of all the projects are finished, or if the only milestones that you can work on will cause you to violate the above rules, you will stop working. Note that you may not be able to finish every project's milestones due to these constraints.

Return the maximum number of weeks you would be able to work on the projects without violating the rules mentioned above.

__Example:__

Example 1:

```text
Input: milestones = [1,2,3]
Output: 6
Explanation: One possible scenario is:
```

- During the 1st week, you will work on a milestone of project 0.
- During the 2nd week, you will work on a milestone of project 2.
- During the 3rd week, you will work on a milestone of project 1.
- During the 4th week, you will work on a milestone of project 2.
- During the 5th week, you will work on a milestone of project 1.
- During the 6th week, you will work on a milestone of project 2.
The total number of weeks is 6.

Example 2:

```text
Input: milestones = [5,2,1]
Output: 7
Explanation: One possible scenario is:
```

- During the 1st week, you will work on a milestone of project 0.
- During the 2nd week, you will work on a milestone of project 1.
- During the 3rd week, you will work on a milestone of project 0.
- During the 4th week, you will work on a milestone of project 1.
- During the 5th week, you will work on a milestone of project 0.
- During the 6th week, you will work on a milestone of project 2.
- During the 7th week, you will work on a milestone of project 0.
The total number of weeks is 7.
Note that you cannot work on the last milestone of project 0 on 8th week because it would violate the rules.
Thus, one milestone in project 0 will remain unfinished.

__Constraints:__

- `n == milestones.length`
- `1 <= n <= 10 ^ 5`
- `1 <= milestones[i] <= 10 ^ 9`

__题目描述:__

给你 `n` 个项目，编号从 `0` 到 `n - 1` 。同时给你一个整数数组 `milestones` ，其中每个 `milestones[i]` 表示第 `i` 个项目中的阶段任务数量。

你可以按下面两个规则参与项目中的工作：

- 每周，你将会完成 __某一个__ 项目中的 __恰好一个__ 阶段任务。你每周都 __必须__ 工作。
- 在 __连续的__ 两周中，你 __不能__ 参与并完成同一个项目中的两个阶段任务。

一旦所有项目中的全部阶段任务都完成，或者仅剩余一个阶段任务都会导致你违反上面的规则，那么你将 停止工作 。注意，由于这些条件的限制，你可能无法完成所有阶段任务。

返回在不违反上面规则的情况下你 最多 能工作多少周。

__示例:__

示例 1：

```text
输入：milestones = [1,2,3]
输出：6
解释：一种可能的情形是：
```

- 第 1 周，你参与并完成项目 0 中的一个阶段任务。
- 第 2 周，你参与并完成项目 2 中的一个阶段任务。
- 第 3 周，你参与并完成项目 1 中的一个阶段任务。
- 第 4 周，你参与并完成项目 2 中的一个阶段任务。
- 第 5 周，你参与并完成项目 1 中的一个阶段任务。
- 第 6 周，你参与并完成项目 2 中的一个阶段任务。
总周数是 6 。

示例 2：

```text
输入：milestones = [5,2,1]
输出：7
解释：一种可能的情形是：
```

- 第 1 周，你参与并完成项目 0 中的一个阶段任务。
- 第 2 周，你参与并完成项目 1 中的一个阶段任务。
- 第 3 周，你参与并完成项目 0 中的一个阶段任务。
- 第 4 周，你参与并完成项目 1 中的一个阶段任务。
- 第 5 周，你参与并完成项目 0 中的一个阶段任务。
- 第 6 周，你参与并完成项目 2 中的一个阶段任务。
- 第 7 周，你参与并完成项目 0 中的一个阶段任务。
总周数是 7 。
注意，你不能在第 8 周参与完成项目 0 中的最后一个阶段任务，因为这会违反规则。
因此，项目 0 中会有一个阶段任务维持未完成状态。

__提示：__

- `n == milestones.length`
- `1 <= n <= 10 ^ 5`
- `1 <= milestones[i] <= 10 ^ 9`

__思路:__

```text
贪心
每次优先选择最多的任务 max_value
计算数组总和 s
记 rest = s - max_value
如果 max_value > rest + 1, 那么可以完成的任务数为 rest * 2 + 1, 即交替完成 max_value 和 rest 个任务, 最后再完成一个 max_value 个任务
否则可以完成的任务数为 s, 即所有任务都可以完成
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long numberOfWeeks(vector<int>& milestones) 
    {
        long long max_value = *max_element(milestones.begin(), milestones.end()), s = accumulate(milestones.begin(), milestones.end(), 0LL), rest = s - max_value;
        return (max_value > rest + 1) ? (rest << 1) + 1 : s;
    }
};
```

__Java__:

```Java
class Solution {
    public long numberOfWeeks(int[] milestones) {
        long maxValue = 0, rest = 0, s = 0;
        for (int milestone : milestones) {
            s += milestone;
            maxValue = Math.max(milestone, maxValue);
        }
        rest = s - maxValue;
        return (maxValue > rest + 1) ? (rest << 1) + 1 : s;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfWeeks(self, milestones: List[int]):
        return (rest << 1) + 1 if (max_value := max(milestones)) > (rest := sum(milestones) - max_value) + 1 else max_value + rest
```
