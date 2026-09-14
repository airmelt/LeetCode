# 3169 Count Days Without Meetings 无需开会的工作日

__Description:__

You are given a positive integer `days` representing the total number of days an employee is available for work (starting from day 1). You are also given a 2D array `meetings` of size `n` where, `meetings[i] = [start_i, end_i]` represents the starting and ending days of meeting `i` (inclusive).

Return the count of days when the employee is available for work but no meetings are scheduled.

__Note:__ The meetings may overlap.

__Example:__

Example 1:

```text
Input: days = 10, meetings = [[5,7],[1,3],[9,10]]

Output: 2

Explanation:

There is no meeting scheduled on the 4th and 8th days.
```

Example 2:

```text
Input: days = 5, meetings = [[2,4],[1,3]]

Output: 1

Explanation:

There is no meeting scheduled on the 5th day.
```

Example 3:

```text
Input: days = 6, meetings = [[1,6]]

Output: 0

Explanation:

Meetings are scheduled for all working days.
```

__Constraints:__

- `1 <= days <= 10 ^ 9`
- `1 <= meetings.length <= 10 ^ 5`
- `meetings[i].length == 2`
- `1 <= meetings[i][0] <= meetings[i][1] <= days`

__题目描述:__

给你一个正整数 `days`，表示员工可工作的总天数（从第 1 天开始）。另给你一个二维数组 `meetings`，长度为 `n`，其中 `meetings[i] = [start_i, end_i]` 表示第 `i` 次会议的开始和结束天数（包含首尾）。

返回员工可工作且没有安排会议的天数。

__注意：__ 会议时间可能会有重叠。

__示例:__

示例 1：

```text
输入：days = 10, meetings = [[5,7],[1,3],[9,10]]

输出：2

解释：

第 4 天和第 8 天没有安排会议。
```

示例 2：

```text
输入：days = 5, meetings = [[2,4],[1,3]]

输出：1

解释：

第 5 天没有安排会议。
```

示例 3：

```text
输入：days = 6, meetings = [[1,6]]

输出：0

解释：

所有工作日都安排了会议。
```

__提示：__

- `1 <= days <= 10 ^ 9`
- `1 <= meetings.length <= 10 ^ 5`
- `meetings[i].length == 2`
- `1 <= meetings[i][0] <= meetings[i][1] <= days`

__思路:__

```text
合并区间
对所有区间按照开始时间进行排序
从第一个区间开始如果上一次的结束时间比
这一次的开始时间更晚
那么可以新开一个区间
将之前的区间长度从 days 中减去
新的开始时间更新为这次的开始时间
结束时间更新为当前会议的结束时间的最大值
最后返回 days 减去最后一个区间的长度即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要是排序的时间以及空间
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countDays(int days, vector<vector<int>>& meetings) 
    {
        sort(meetings.begin(), meetings.end());
        int start = 1, end = 0;
        for (const auto& meeting : meetings) 
        {
            if (meeting.front() > end) 
            {
                days -= end - start + 1;
                start = meeting.front();
            }
            end = max(end, meeting[1]);
        }
        return days - end + start - 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int countDays(int days, int[][] meetings) {
        Arrays.sort(meetings, (a, b) -> a[0] - b[0]);
        int start = 1, end = 0;
        for (var meeting : meetings) {
            if (meeting[0] > end) {
                days -= end - start + 1;
                start = meeting[0];
            }
            end = Math.max(end, meeting[1]);
        }
        return days - end + start - 1;
    }
}
```

__Python__:

```Python
class Solution:
    def countDays(self, days: int, meetings: List[List[int]]) -> int:
        meetings.sort()
        start, end = 1, 0
        for s, e in meetings:
            if s > end:
                days -= end - start + 1
                start = s
            end = max(end, e)
        return days - end + start - 1
```
