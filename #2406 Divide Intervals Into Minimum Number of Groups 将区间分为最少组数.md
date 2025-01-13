# 2406 Divide Intervals Into Minimum Number of Groups 将区间分为最少组数

__Description:__

You are given a 2D integer array `intervals` where `intervals[i] = [left_i, right_i]` represents the __inclusive__ interval `[left_i, right_i]`.

You have to divide the intervals into one or more groups such that each interval is in exactly one group, and no two intervals that are in the same group intersect each other.

Return the minimum number of groups you need to make.

Two intervals __intersect__ if there is at least one common number between them. For example, the intervals `[1, 5]` and `[5, 8]` intersect.

__Example:__

Example 1:

```text
Input: intervals = [[5,10],[6,8],[1,5],[2,3],[1,10]]
Output: 3
Explanation: We can divide the intervals into the following groups:
```

- Group 1: [1, 5], [6, 8].
- Group 2: [2, 3], [5, 10].
- Group 3: [1, 10].

It can be proven that it is not possible to divide the intervals into fewer than 3 groups.

Example 2:

```text
Input: intervals = [[1,3],[5,6],[8,10],[11,13]]
Output: 1
Explanation: None of the intervals overlap, so we can put all of them in one group.
```

__Constraints:__

- `1 <= intervals.length <= 10 ^ 5`
- `intervals[i].length == 2`
- `1 <= left_i <= right_i <= 10 ^ 6`

__题目描述:__

给你一个二维整数数组 `intervals` ，其中 `intervals[i] = [left_i, right_i]` 表示 __闭__ 区间 `[left_i, right_i]` 。

你需要将 `intervals` 划分为一个或者多个区间 __组__ ，每个区间 _只_ 属于一个组，且同一个组中任意两个区间 __不相交__ 。

请你返回 最少 需要划分成多少个组。

如果两个区间覆盖的范围有重叠（即至少有一个公共数字），那么我们称这两个区间是 __相交__ 的。比方说区间 `[1, 5]` 和 `[5, 8]` 相交。

__示例:__

示例 1：

```text
输入：intervals = [[5,10],[6,8],[1,5],[2,3],[1,10]]
输出：3
解释：我们可以将区间划分为如下的区间组：
```

- 第 1 组：[1, 5] ，[6, 8] 。
- 第 2 组：[2, 3] ，[5, 10] 。
- 第 3 组：[1, 10] 。

可以证明无法将区间划分为少于 3 个组。

示例 2：

```text
输入：intervals = [[1,3],[5,6],[8,10],[11,13]]
输出：1
解释：所有区间互不相交，所以我们可以把它们全部放在一个组内。
```

__提示：__

- `1 <= intervals.length <= 10 ^ 5`
- `intervals[i].length == 2`
- `1 <= left_i <= right_i <= 10 ^ 6`

__思路:__

```text
差分数组
每一个重复的区间都需要单独分组
所以只需要计算出每一个区间的重叠次数，最后返回最大的重叠次数即可
用差分数组统计每一个区间的重叠次数
最后返回差分数组的前缀和的最大值即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minGroups(vector<vector<int>>& intervals) 
    {
        map<int, int> diff;
        for (const auto& i : intervals)
        {
            ++diff[i.front()];
            --diff[i.back() + 1];
        }
        int result = 0, cur = 0;
        for (const auto& [_, d] : diff) result = max(result, cur += d);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minGroups(int[][] intervals) {
        int N = 1_000_002, count[] = new int[N];
        for (int[] interval : intervals) {
            ++count[interval[0]];
            --count[interval[1] + 1];
        }
        for (int i = 1; i < N; i++) count[i] += count[i - 1];
        return Arrays.stream(count).max().getAsInt();
    }
}
```

__Python__:

```Python
class Solution:
    def minGroups(self, intervals: List[List[int]]) -> int:
        count = [0] * (n := 10 ** 6 + 2)
        for i in intervals:
            count[i[0]] += 1
            count[i[1] + 1] -= 1
        for i in range(1, n):
            count[i] += count[i - 1]
        return max(count)
```
