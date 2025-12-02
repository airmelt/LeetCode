# 2798 Number of Employees Who Met the Target 满足目标工作时长的员工数目

__Description:__

There are `n` employees in a company, numbered from `0` to `n - 1`. Each employee `i` has worked for `hours[i]` hours in the company.

The company requires each employee to work for __at least__ `target` hours.

You are given a __0-indexed__ array of non-negative integers `hours` of length `n` and a non-negative integer `target`.

Return _the integer denoting the number of employees who worked at least_ `target` _hours_.

__Example:__

Example 1:

```text
Input: hours = [0,1,2,3,4], target = 2
Output: 3
Explanation: The company wants each employee to work for at least 2 hours.
```

- Employee 0 worked for 0 hours and didn't meet the target.
- Employee 1 worked for 1 hours and didn't meet the target.
- Employee 2 worked for 2 hours and met the target.
- Employee 3 worked for 3 hours and met the target.
- Employee 4 worked for 4 hours and met the target.

There are 3 employees who met the target.

Example 2:

```text
Input: hours = [5,1,4,2,2], target = 6
Output: 0
Explanation: The company wants each employee to work for at least 6 hours.
There are 0 employees who met the target.
```

__Constraints:__

- `1 <= n == hours.length <= 50`
- `0 <= hours[i], target <= 10 ^ 5`

__题目描述:__

公司里共有 `n` 名员工，按从 `0` 到 `n - 1` 编号。每个员工 `i` 已经在公司工作了 `hours[i]` 小时。

公司要求每位员工工作 __至少__ `target` 小时。

给你一个下标从 __0__ 开始、长度为 `n` 的非负整数数组 `hours` 和一个非负整数 `target` 。

请你用整数表示并返回工作至少 `target` 小时的员工数。

__示例:__

示例 1：

```text
输入：hours = [0,1,2,3,4], target = 2
输出：3
解释：公司要求每位员工工作至少 2 小时。
```

- 员工 0 工作 0 小时，不满足要求。
- 员工 1 工作 1 小时，不满足要求。
- 员工 2 工作 2 小时，满足要求。
- 员工 3 工作 3 小时，满足要求。
- 员工 4 工作 4 小时，满足要求。

共有 3 位满足要求的员工。

示例 2：

```text
输入：hours = [5,1,4,2,2], target = 6
输出：0
解释：公司要求每位员工工作至少 6 小时。
共有 0 位满足要求的员工。
```

__提示：__

- `1 <= n == hours.length <= 50`
- `0 <= hours[i], target <= 10 ^ 5`

__思路:__

```text
模拟
遍历所有元素
找到不小于 target 的元素即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfEmployeesWhoMetTarget(vector<int>& hours, int target) 
    {
        return std::count_if(hours.begin(), hours.end(), [&](const auto& h) -> bool { return h >= target; });
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfEmployeesWhoMetTarget(int[] hours, int target) {
        return (int)Arrays.stream(hours).filter(h -> h >= target).count();
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfEmployeesWhoMetTarget(self, hours: List[int], target: int) -> int:
        return sum(i >= target for i in hours)
```
