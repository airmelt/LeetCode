# 56 Merge Intervals 合并区间

__Description__:
Given a collection of intervals, merge all overlapping intervals.

__Example:__

Example 1:

Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].

Example 2:

Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.

__NOTE:__
 input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

__题目描述__:
给出一个区间的集合，请合并所有重叠的区间。

__示例 :__

示例 1:

输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].

示例 2:

输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。

__思路__:

1. 排序, 将 intervals数组中的第一个元素排序
2. 遍历intervals数组
3. 如果结果数组为空数组或者结果数组中的最后一个元素的右侧比 intervals当前元素的左侧还要小, 加入结果数组
4. 否则比较结果数组中的最后一个元素的右侧和 intervals当前元素的右侧, 取较大的元素
时间复杂度O(nlgn), 空间复杂度O(lgn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) 
    {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> result;
        for (auto &i : intervals)
        {
            if (result.empty() or i[0] > result.back()[1]) result.push_back(i);
            else result.back()[1] = max(result.back()[1], i[1]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        int result[][] = new int[intervals.length][2], i = -1;
        for (int[] interval: intervals) {
            if (i == -1 || interval[0] > result[i][1]) result[++i] = interval;
            else result[i][1] = Math.max(result[i][1], interval[1]);
        }
        return Arrays.copyOf(result, i + 1);
    }
}
```

__Python__:

```Python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        result = []
        intervals.sort()
        for i in intervals:
            if not result or result[-1][1] < i[0]:
                result.append(i)
            else:
                result[-1][1] = max(result[-1][1], i[1])
        return result
```
