# 757 Set Intersection Size At Least Two 设置交集大小至少为2

__Description__:
An integer interval [a, b] (for integers a < b) is a set of all consecutive integers from a to b, including a and b.

Find the minimum size of a set S such that for every integer interval A in intervals, the intersection of S with A has a size of at least two.

__Example:__

Example 1:

Input: intervals = [[1,3],[1,4],[2,5],[3,5]]
Output: 3
Explanation: Consider the set S = {2, 3, 4}.  For each interval, there are at least 2 elements from S in the interval.
Also, there isn't a smaller size set that fulfills the above condition.
Thus, we output the size of this set, which is 3.

Example 2:

Input: intervals = [[1,2],[2,3],[2,4],[4,5]]
Output: 5
Explanation: An example of a minimum sized set is {1, 2, 3, 4, 5}.

__Constraints:__

1 <= intervals.length <= 3000
intervals[i].length == 2
0 <= ai < bi <= 10^8

__题目描述__:
一个整数区间 [a, b]  ( a < b ) 代表着从 a 到 b 的所有连续整数，包括 a 和 b。

给你一组整数区间intervals，请找到一个最小的集合 S，使得 S 里的元素与区间intervals中的每一个整数区间都至少有2个元素相交。

输出这个最小集合S的大小。

__示例 :__

示例 1:

输入: intervals = [[1, 3], [1, 4], [2, 5], [3, 5]]
输出: 3
解释:
考虑集合 S = {2, 3, 4}. S与intervals中的四个区间都有至少2个相交的元素。
且这是S最小的情况，故我们输出3。

示例 2:

输入: intervals = [[1, 2], [2, 3], [2, 4], [4, 5]]
输出: 5
解释:
最小的集合S = {1, 2, 3, 4, 5}.

__注意:__

intervals 的长度范围为[1, 3000]。
intervals[i] 长度为 2，分别代表左、右边界。
intervals[i][j] 的值是 [0, 10^8]范围内的整数。

__思路__:

贪心
先按照区间右边界升序排序, 再按左边界降序排序, 这样就能保证在右边界相同的时候后面的区间是前面区间的子区间
记结果中最大的区间为 [left, right] = [result[-2], result[-1]], 这里结果数组一定也是有序的
对每个区间

1. 如果当前区间 interval[i] 和 result 完全没有交集, 注意到 result 一定是从小遍历到大的区间的, 此时, 当前区间一定在 [left, right] 的右边, 即 result[-1] < interval[i][0], 考虑到后面的交集应该是更大的数, 为了取出两个数能够使得与后面交集的概率更大, 应该取当前区间中最大的两个数
2. 如果当前区间 interval[i] 和 result 的交集有且只有一个元素, 此时 result[-1] == interval[0], 这是因为前面区间最大也只能取到 interval[i - 1][1], 又因为 intervals 有序, 所以有 interval[i][1] > interval[i - 1][1] && interval[i][0] == interval[i - 1][1], 这时将 interval[i][1] 加入结果数组
3. 如果当前区间 interval[i] 和 result 的交集有多个元素, 不用进行操作
注意到只需要结果数组的长度, 用 left, right 记录区间最大的两个数字, result 只需要记录长度即可
时间复杂度为 O(nlgn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int intersectionSizeTwo(vector<vector<int>>& intervals) 
    {
        sort(intervals.begin(), intervals.end(), [](const auto& a, const auto& b) { return a.back() == b.back() ? a.front() > b.front() : a.back() < b.back(); });
        int result = 0, left = -1, right = -1;
        for (const auto& interval : intervals)
        {
            if (interval.front() <= left) continue;
            if (right < interval.front()) 
            {
                result += 2;
                right = interval.back();
                left = right - 1;
            } 
            else 
            {
                ++result;
                left = right;
                right = interval.back();
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int intersectionSizeTwo(int[][] intervals) {
        Arrays.sort(intervals, (int[] a, int[] b) -> { return a[1] == b[1] ? b[0] - a[0] : a[1] - b[1]; });
        int result = 0, left = -1, right = -1;
        for (int interval[] : intervals) {
            if (interval[0] <= left) continue;
            if (right < interval[0]) {
                result += 2;
                right = interval[1];
                left = right - 1;
            } else {
                ++result;
                left = right;
                right = interval[1];
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def intersectionSizeTwo(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda x: (x[1], -x[0]))
        result = [-1, -1]
        for i in intervals:
            i[0] > result[-1] and result.append(i[1] - 1) or (i[0] > result[-2] and  result.append(i[1]))
        return len(result) - 2
```
