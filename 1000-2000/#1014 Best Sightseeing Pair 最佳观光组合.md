# 1014 Best Sightseeing Pair 最佳观光组合

__Description__:
You are given an integer array values where values[i] represents the value of the ith sightseeing spot. Two sightseeing spots i and j have a distance j - i between them.

The score of a pair (i < j) of sightseeing spots is values[i] + values[j] + i - j: the sum of the values of the sightseeing spots, minus the distance between them.

Return the maximum score of a pair of sightseeing spots.

__Example:__

Example 1:

Input: values = [8,1,5,2,6]
Output: 11
Explanation: i = 0, j = 2, values[i] + values[j] + i - j = 8 + 5 + 0 - 2 = 11

Example 2:

Input: values = [1,2]
Output: 2

__Constraints:__

2 <= values.length <= 5 * 10^4
1 <= values[i] <= 1000

__题目描述__:
给你一个正整数数组 values，其中 values[i] 表示第 i 个观光景点的评分，并且两个景点 i 和 j 之间的 距离 为 j - i。

一对景点（i < j）组成的观光组合的得分为 values[i] + values[j] + i - j ，也就是景点的评分之和 减去 它们两者之间的距离。

返回一对观光景点能取得的最高分。

__示例 :__

示例 1：

输入：values = [8,1,5,2,6]
输出：11
解释：i = 0, j = 2, values[i] + values[j] + i - j = 8 + 5 + 0 - 2 = 11

示例 2：

输入：values = [1,2]
输出：2

__提示:__

2 <= values.length <= 5 * 10^4
1 <= values[i] <= 1000

__思路__:

动态规划
values[i] + values[j] + i - j -> values[i] + i + values[j] - j
左边选择一个 values[i] + i 的最大值 left
右边选择一个 left + values[j] - j 的最大值
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxScoreSightseeingPair(vector<int>& values) 
    {
        int left = values.front(), result = -1, n = values.size();
        for (int i = 1; i < n; i++) 
        {
            result = max(result, left + values[i] - i);
            left = max(left, values[i] + i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxScoreSightseeingPair(int[] values) {
        int left = values[0], result = -1, n = values.length;
        for (int i = 1; i < n; i++) {
            result = Math.max(result, left + values[i] - i);
            left = Math.max(left, values[i] + i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxScoreSightseeingPair(self, values: List[int]) -> int:
        result, left, n = -1, values[0], len(values)
        for i in range(1, n):
            result, left = max(result, left + values[i] - i), max(left, values[i] + i)
        return result
```
