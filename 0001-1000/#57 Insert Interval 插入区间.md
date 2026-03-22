# 57 Insert Interval 插入区间

__Description__:
Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

__Example:__

Example 1:

Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]

Example 2:

Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

__NOTE:__
 input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

__题目描述__:
给出一个无重叠的 ，按照区间起始端点排序的区间列表。

在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。

__示例 :__

示例 1:

输入: intervals = [[1,3],[6,9]], newInterval = [2,5]
输出: [[1,5],[6,9]]

示例 2:

输入: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
输出: [[1,2],[3,10],[12,16]]
解释: 这是因为新的区间 [4,8] 与 [3,5],[6,7],[8,10] 重叠。

__思路__:

1. 遍历 intervals数组, 将 newInterval左边的区间加入结果数组
2. 合并区间, 将合并的结果加入结果数组
3. 将 intervals数组剩下的元素加入结果数组
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) 
    {
        int i = 0;
        vector<vector<int>> result;
        while (i < intervals.size() and newInterval[0] > intervals[i][1]) result.push_back(intervals[i++]);
        while (i < intervals.size() and newInterval[1] >= intervals[i][0]) 
        {
            newInterval[0] = min(newInterval[0], intervals[i][0]);
            newInterval[1] = max(newInterval[1], intervals[i++][1]);
        }
        result.push_back(newInterval);
        while (i < intervals.size()) result.push_back(intervals[i++]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        int i = 0, j = 0, result[][] = new int[intervals.length == 0 ? 1 : intervals.length + 1][2];
        while (i < intervals.length && newInterval[0] > intervals[i][1]) {
            result[j][0] = intervals[i][0];
            result[j++][1] = intervals[i++][1];
        }
        while (i < intervals.length && newInterval[1] >= intervals[i][0]) {
            newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
            newInterval[1] = Math.max(newInterval[1], intervals[i++][1]);
        }
        result[j][0] = newInterval[0];
        result[j++][1] = newInterval[1];
        while (i < intervals.length) {
            result[j][0] = intervals[i][0];
            result[j++][1] = intervals[i++][1];
        }
        return Arrays.copyOf(result, j);
    }
}
```

__Python__:

```Python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        result, i = [], 0
        while i < len(intervals) and newInterval[0] > intervals[i][1]:
            result.append(intervals[i])
            i += 1
        while i < len(intervals) and newInterval[1] >= intervals[i][0]:
            newInterval[0],  newInterval[1] = min(newInterval[0], intervals[i][0]), max(newInterval[1], intervals[i][1])
            i += 1
        result.append(newInterval)
        while i < len(intervals):
            result.append(intervals[i])
            i += 1
        return result
```
