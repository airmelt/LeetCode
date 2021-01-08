# 436 Find Right Interval 寻找右区间

__Description__:
You are given an array of intervals, where intervals[i] = [start_i, end_i] and each start_i is unique.

The right interval for an interval i is an interval j such that start_j >= end_i and start_j is minimized.

Return an array of right interval indices for each interval i. If no right interval exists for interval i, then put -1 at index i.

__Example:__

Example 1:

Input: intervals = [[1,2]]
Output: [-1]
Explanation: There is only one interval in the collection, so it outputs -1.

Example 2:

Input: intervals = [[3,4],[2,3],[1,2]]
Output: [-1,0,1]
Explanation: There is no right interval for [3,4].
The right interval for [2,3] is [3,4] since start0 = 3 is the smallest start that is >= end1 = 3.
The right interval for [1,2] is [2,3] since start1 = 2 is the smallest start that is >= end2 = 2.

Example 3:

Input: intervals = [[1,4],[2,3],[3,4]]
Output: [-1,2,-1]
Explanation: There is no right interval for [1,4] and [3,4].
The right interval for [2,3] is [3,4] since start2 = 3 is the smallest start that is >= end1 = 3.

__Constraints:__

1 <= intervals.length <= 2 * 10^4
intervals[i].length == 2
-10^6 <= starti <= endi <= 10^6
The start point of each interval is unique.

__题目描述__:
给定一组区间，对于每一个区间 i，检查是否存在一个区间 j，它的起始点大于或等于区间 i 的终点，这可以称为 j 在 i 的“右侧”。

对于任何区间，你需要存储的满足条件的区间 j 的最小索引，这意味着区间 j 有最小的起始点可以使其成为“右侧”区间。如果区间 j 不存在，则将区间 i 存储为 -1。最后，你需要输出一个值为存储的区间值的数组。

__注意：__

你可以假设区间的终点总是大于它的起始点。
你可以假定这些区间都不具有相同的起始点。

__示例 :__

示例 1:

输入: [ [1,2] ]
输出: [-1]

解释:集合中只有一个区间，所以输出-1。

示例 2:

输入: [ [3,4], [2,3], [1,2] ]
输出: [-1, 0, 1]

解释:对于[3,4]，没有满足条件的“右侧”区间。
对于[2,3]，区间[3,4]具有最小的“右”起点;
对于[1,2]，区间[2,3]具有最小的“右”起点。

示例 3:

输入: [ [1,4], [2,3], [3,4] ]
输出: [-1, 2, -1]

解释:对于区间[1,4]和[3,4]，没有满足条件的“右侧”区间。
对于[2,3]，区间[3,4]有最小的“右”起点。

__思路__:

对区间的左端点进行排序, 排序之后可以用二分查找查询
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> findRightInterval(vector<vector<int>>& intervals) 
    {
        map<int, int> m;
        vector<int> result;
        for (int i = 0; i < intervals.size(); i++) m[intervals[i].front()] = i;
        for (auto interval : intervals) result.emplace_back(m.lower_bound(interval.back()) == m.end() ? -1 : m.lower_bound(interval.back()) -> second);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findRightInterval(int[][] intervals) {
        int count[][] = new int[intervals.length][2], n = intervals.length, result[] = new int[intervals.length], m = 0;
        for (int i = 0; i < n; i++) {
            count[i][0] = intervals[i][0];
            count[i][1] = i;
        }
        Arrays.sort(count, new Comparator<int[]>(){ public int compare (int[] a, int[] b) { return a[0] - b[0]; } });
        for (int interval[] : intervals) {
            int target = interval[1], l = 0, r = n;
            while (l < r) {
                int mid = (l + r) >> 1;
                if (count[mid][0] < target) l = mid + 1;
                else r = mid;
            }
            result[m++] = (l == n ? -1 : count[l][1]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findRightInterval(self, intervals: List[List[int]]) -> List[int]:
        sorted_intervals, result = sorted([(interval[0], i) for i, interval in enumerate(intervals)]), []
        for interval in intervals:
            target, l, r = interval[1], 0, len(intervals) 
            while l < r:
                mid = (l + r) >> 1
                if sorted_intervals[mid][0] < target:
                    l = mid + 1
                else:
                    r = mid
            result.append(-1 if l == len(intervals) else sorted_intervals[l][1])
        return result
```
