# 986 Interval List Intersections 区间列表的交集

__Description__:
You are given two lists of closed intervals, firstList and secondList, where firstList[i] = [starti, endi] and secondList[j] = [startj, endj]. Each list of intervals is pairwise disjoint and in sorted order.

Return the intersection of these two interval lists.

A closed interval [a, b] (with a <= b) denotes the set of real numbers x with a <= x <= b.

The intersection of two closed intervals is a set of real numbers that are either empty or represented as a closed interval. For example, the intersection of [1, 3] and [2, 4] is [2, 3].

__Example:__

Example 1:

![interval](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)

Input: firstList = [[0,2],[5,10],[13,23],[24,25]], secondList = [[1,5],[8,12],[15,24],[25,26]]
Output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]

Example 2:

Input: firstList = [[1,3],[5,9]], secondList = []
Output: []

__Constraints:__

0 <= firstList.length, secondList.length <= 1000
firstList.length + secondList.length >= 1
0 <= starti < endi <= 10^9
endi < starti+1
0 <= startj < endj <= 10^9
endj < startj+1

__题目描述__:
给定两个由一些 闭区间 组成的列表，firstList 和 secondList ，其中 firstList[i] = [starti, endi] 而 secondList[j] = [startj, endj] 。每个区间列表都是成对 不相交 的，并且 已经排序 。

返回这 两个区间列表的交集 。

形式上，闭区间 [a, b]（其中 a <= b）表示实数 x 的集合，而 a <= x <= b 。

两个闭区间的 交集 是一组实数，要么为空集，要么为闭区间。例如，[1, 3] 和 [2, 4] 的交集为 [2, 3] 。

__示例 :__

示例 1：

![区间列表的交集](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)

输入：firstList = [[0,2],[5,10],[13,23],[24,25]], secondList = [[1,5],[8,12],[15,24],[25,26]]
输出：[[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]

示例 2：

输入：firstList = [[1,3],[5,9]], secondList = []
输出：[]

示例 3：

输入：firstList = [], secondList = [[4,8],[10,12]]
输出：[]

示例 4：

输入：firstList = [[1,7]], secondList = [[3,10]]
输出：[[3,7]]

__提示:__

0 <= firstList.length, secondList.length <= 1000
firstList.length + secondList.length >= 1
0 <= starti < endi <= 10^9
endi < starti+1
0 <= startj < endj <= 10^9
endj < startj+1

__思路__:

双指针
区间已经有序所以交集左边界一定是 max(firstList[i][0], secondList[j][0]), 右边界一定是 min(firstList[i][1], secondList[j][1])
如果相交 right >= left
否则不相交, 这时需要移动指针, 比较 firstList[i][1], secondList[j][1], 较小的指针往后移
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> intervalIntersection(vector<vector<int>>& firstList, vector<vector<int>>& secondList) 
    {
        int i = 0, j = 0, m = firstList.size(), n = secondList.size();
        vector<vector<int>> result;
        while (i < m and j < n) 
        {
            int left = max(firstList[i].front(), secondList[j].front()), right = min(firstList[i].back(), secondList[j].back());
            if (right >= left) result.emplace_back(vector<int>{left, right});
            if (firstList[i].back() < secondList[j].back()) ++i;
            else ++j;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        int i = 0, j = 0, m = firstList.length, n = secondList.length;
        List<int[]> result = new ArrayList<>();
        while (i < m && j < n) {
            int left = Math.max(firstList[i][0], secondList[j][0]), right = Math.min(firstList[i][1], secondList[j][1]);
            if (right >= left) result.add(new int[]{left, right});
            if (firstList[i][1] < secondList[j][1]) ++i;
            else ++j;
        }
        return (int[][])result.toArray(new int[0][]);
    }
}
```

__Python__:

```Python
class Solution:
    def intervalIntersection(self, firstList: List[List[int]], secondList: List[List[int]]) -> List[List[int]]:
        result, m, n, i, j = [], len(firstList), len(secondList), 0, 0
        while i < m and j < n:
            if (right := min(firstList[i][1], secondList[j][1])) >= (left := max(firstList[i][0], secondList[j][0])):
                result.append([left, right])
            if firstList[i][1] < secondList[j][1]:
                i += 1
            else:
                j += 1
        return result
```
