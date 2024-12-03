# 2358 Maximum Number of Groups Entering a Competition 分组的最大数量

__Description:__

You are given a positive integer array `grades` which represents the grades of students in a university. You would like to enter __all__ these students into a competition in __ordered__ non-empty groups, such that the ordering meets the following conditions:

- The sum of the grades of students in the `i ^ th` group is __less than__ the sum of the grades of students in the `(i + 1) ^ th` group, for all groups (except the last).
- The total number of students in the `i ^ th` group is __less than__ the total number of students in the `(i + 1) ^ th` group, for all groups (except the last).

Return the maximum number of groups that can be formed.

__Example:__

Example 1:

```text
Input: grades = [10,6,12,7,3,5]
Output: 3
Explanation: The following is a possible way to form 3 groups of students:
```

- 1st group has the students with grades = [12]. Sum of grades: 12. Student count: 1
- 2nd group has the students with grades = [6,7]. Sum of grades: 6 + 7 = 13. Student count: 2
- 3rd group has the students with grades = [10,3,5]. Sum of grades: 10 + 3 + 5 = 18. Student count: 3

It can be shown that it is not possible to form more than 3 groups.

Example 2:

```text
Input: grades = [8,8]
Output: 1
Explanation: We can only form 1 group, since forming 2 groups would lead to an equal number of students in both groups.
```

__Constraints:__

- `1 <= grades.length <= 10 ^ 5`
- `1 <= grades[i] <= 10 ^ 5`

__题目描述:__

给你一个正整数数组 `grades` ，表示大学中一些学生的成绩。你打算将 __所有__ 学生分为一些 __有序__ 的非空分组，其中分组间的顺序满足以下全部条件:

- 第 `i` 个分组中的学生总成绩 __小于__ 第 `(i + 1)` 个分组中的学生总成绩，对所有组均成立（除了最后一组）。
- 第 `i` 个分组中的学生总数 __小于__ 第 `(i + 1)` 个分组中的学生总数，对所有组均成立（除了最后一组）。

返回可以形成的 最大 组数。

__示例:__

示例 1：

```text
输入：grades = [10,6,12,7,3,5]
输出：3
解释：下面是形成 3 个分组的一种可行方法：
```

- 第 1 个分组的学生成绩为 grades = [12] ，总成绩：12 ，学生数：1
- 第 2 个分组的学生成绩为 grades = [6,7] ，总成绩：6 + 7 = 13 ，学生数：2
- 第 3 个分组的学生成绩为 grades = [10,3,5] ，总成绩：10 + 3 + 5 = 18 ，学生数：3

可以证明无法形成超过 3 个分组。

示例 2：

```text
输入：grades = [8,8]
输出：1
解释：只能形成 1 个分组，因为如果要形成 2 个分组的话，会导致每个分组中的学生数目相等。
```

__提示：__

- `1 <= grades.length <= 10 ^ 5`
- `1 <= grades[i] <= 10 ^ 5`

__思路:__

```text
贪心
由于前一分组的人数和成绩都要小于后一分组
实际上我们可以选择排序好的就依次分给各个组
问题就转化成 1 + 2 + 3 + ... + x <= n
求解 x 的最大值
由等差数列求和公式可得 x = (-1 + sqrt(1 + 8n)) / 2
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumGroups(vector<int>& grades) 
    {
        return ((-1 + sqrt(1 + (grades.size() << 3))) / 2.0);
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumGroups(int[] grades) {
        return (int)((-1 + Math.sqrt(1 + (grades.length << 3))) / 2.0);
    }
}
```

__Python__:

```Python
class Solution:
    def maximumGroups(self, grades: List[int]) -> int:
        return int((-1 + (1 + (len(grades) << 3)) ** 0.5) / 2.0)
```
