# 1288 Remove Covered Intervals 删除被覆盖区间

__Description:__

Given an array intervals where intervals[i] = [li, ri] represent the interval [li, ri), remove all intervals that are covered by another interval in the list.

The interval [a, b) is covered by the interval [c, d) if and only if c <= a and b <= d.

Return the number of remaining intervals.

__Example:__

Example 1:

Input: intervals = [[1,4],[3,6],[2,8]]
Output: 2
Explanation: Interval [3,6] is covered by [2,8], therefore it is removed.

Example 2:

Input: intervals = [[1,4],[2,3]]
Output: 1

__Constraints:__

1 <= intervals.length <= 1000
intervals[i].length == 2
0 <= li < ri <= 10^5
All the given intervals are unique.

__题目描述：__

给你一个区间列表，请你删除列表中被其他区间所覆盖的区间。

只有当 c <= a 且 b <= d 时，我们才认为区间 [a,b) 被区间 [c,d) 覆盖。

在完成所有删除操作后，请你返回列表中剩余区间的数目。

__示例：__

输入：intervals = [[1,4],[3,6],[2,8]]
输出：2
解释：区间 [3,6] 被区间 [2,8] 覆盖，所以它被删除了。

__提示：__

1 <= intervals.length <= 1000
0 <= intervals[i][0] < intervals[i][1] <= 10^5
对于所有的 i != j：intervals[i] != intervals[j]

__思路：__

贪心
按照区间起点升序, 终点降序对区间进行排序
那么排序之后起点相同的终点排在前面的更大
记录前一个更大的终点为 pre
如果当前区间终点比 pre 小, 说明需要删除
否则更新 pre 为当前区间终点
时间复杂度为 O(nlgn), 空间复杂度为 O(lgn), 主要为排序所需时间及空间

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int removeCoveredIntervals(vector<vector<int>>& intervals) 
    {
        sort(intervals.begin(), intervals.end(), [&](const auto& a, const auto& b) { return a.front() == b.front() ? a.back() > b.back() : a.front() < b.front(); });
        int del = 0, n = intervals.size(), pre = intervals.front().back();
        for (int i = 1; i < n; i++)
        {
            if (intervals[i][1] <= pre) ++del;
            else pre = intervals[i][1];
        }
        return n - del;
    }
};
```

__Java__:

```Java
class Solution {
    public int removeCoveredIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        int delete = 0, n = intervals.length, pre = intervals[0][1];
        for (int i = 1; i < n; i++) {
            if (intervals[i][1] <= pre) ++delete;
            else pre = intervals[i][1];
        }
        return n - delete;
    }
}
```

__Python__:

```Python
class Solution:
    def removeCoveredIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda x: (x[0], -x[1]))
        delete, n, pre = 0, len(intervals), intervals[0][1]
        for i in range(1, n):
            if intervals[i][1] <= pre:
                delete += 1
            else:
                pre = intervals[i][1]
        return n - delete
```
