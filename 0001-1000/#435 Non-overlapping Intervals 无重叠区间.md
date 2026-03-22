# 435 Non-overlapping Intervals 无重叠区间

__Description__:
Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

__Example:__

Example 1:

Input: [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of intervals are non-overlapping.

Example 2:

Input: [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of intervals non-overlapping.

Example 3:

Input: [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.

__Note:__

You may assume the interval's end point is always bigger than its start point.
Intervals like [1,2] and [2,3] have borders "touching" but they don't overlap each other.

__题目描述__:
给定一个区间的集合，找到需要移除区间的最小数量，使剩余区间互不重叠。

__注意：__

可以认为区间的终点总是大于它的起点。
区间 [1,2] 和 [2,3] 的边界相互“接触”，但没有相互重叠。

__示例 :__

示例 1:

输入: [ [1,2], [2,3], [3,4], [1,3] ]

输出: 1

解释: 移除 [1,3] 后，剩下的区间没有重叠。

示例 2:

输入: [ [1,2], [1,2], [1,2] ]

输出: 2

解释: 你需要移除两个 [1,2] 来使剩下的区间没有重叠。

示例 3:

输入: [ [1,2], [2,3] ]

输出: 0

解释: 你不需要移除任何区间，因为它们已经是无重叠的了。

__思路__:

按照区间的右端点排序, 取第一个的右端点, 如果下一个端点的左端点在上一个端点的左边, 说明要去掉这个区间, 否则更新新的右端点
时间复杂度O(nlgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) 
    {
        if (intervals.empty()) return 0;
        sort(intervals.begin(), intervals.end(), [](auto &a, auto &b) { return a.back() < b.back(); });
        int count = 0, right = intervals[0].back();
        for (int i = 1; i < intervals.size(); i++) 
        {
            if (right > intervals[i].front()) ++count;
            else right = intervals[i].back();
        }
        return count;
    }
};
```

__Java__:

```Java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length == 0) return 0;
        Arrays.sort(intervals, new Comparator<int[]>() { public int compare(int[] a, int[] b) { return a[1] - b[1]; } });
        int count = 0, right = intervals[0][1];
        for (int i = 1; i < intervals.length; i++) {
            if (right > intervals[i][0]) ++count;
            else right = intervals[i][1];
        }
        return count;
    }
}
```

__Python__:

```Python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        if not intervals:
            return 0
        intervals.sort(key = lambda x : x[1])
        count, n, right = 0, len(intervals), intervals[0][1]
        for i in range(1, n):
            if right > intervals[i][0]:
                count += 1
            else:
                right = intervals[i][1]
        return count
```
