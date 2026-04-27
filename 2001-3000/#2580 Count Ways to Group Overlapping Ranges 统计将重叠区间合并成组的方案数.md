# 2580 Count Ways to Group Overlapping Ranges 统计将重叠区间合并成组的方案数

__Description:__

You are given a 2D integer array `ranges` where `ranges[i] = [start_i, end_i]` denotes that all integers between `start_i` and `end_i` (both __inclusive__) are contained in the `i ^ th` range.

You are to split `ranges` into __two__ (possibly empty) groups such that:

- Each range belongs to exactly one group.
- Any two __overlapping__ ranges must belong to the __same__ group.

Two ranges are said to be overlapping if there exists at least one integer that is present in both ranges.

- For example, `[1, 3]` and `[2, 5]` are overlapping because `2` and `3` occur in both ranges.

Return _the __total number__ of ways to split_ `ranges` _into two groups_. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: ranges = [[6,10],[5,15]]
Output: 2
Explanation: 
The two ranges are overlapping, so they must be in the same group.
Thus, there are two possible ways:
```

- Put both the ranges together in group 1.
- Put both the ranges together in group 2.

Example 2:

```text
Input: ranges = [[1,3],[10,20],[2,5],[4,8]]
Output: 4
Explanation: 
Ranges [1,3], and [2,5] are overlapping. So, they must be in the same group.
Again, ranges [2,5] and [4,8] are also overlapping. So, they must also be in the same group. 
Thus, there are four possible ways to group them:
```

- All the ranges in group 1.
- All the ranges in group 2.
- Ranges [1,3], [2,5], and [4,8] in group 1 and [10,20] in group 2.

- Ranges [1,3], [2,5], and [4,8] in group 2 and [10,20] in group 1.

__Constraints:__

- `1 <= ranges.length <= 10 ^ 5`
- `ranges[i].length == 2`
- `0 <= start_i <= end_i <= 10 ^ 9`

__题目描述:__

给你一个二维整数数组 `ranges` ，其中 `ranges[i] = [start_i, end_i]` 表示 `start_i` 到 `end_i` 之间（包括二者）的所有整数都包含在第 `i` 个区间中。

你需要将 `ranges` 分成 __两个__ 组（可以为空），满足:

- 每个区间只属于一个组。
- 两个有 __交集__ 的区间必须在 __同一个__ 组内。

如果两个区间有至少 一个 公共整数，那么这两个区间是 有交集 的。

- 比方说，区间 `[1, 3]` 和 `[2, 5]` 有交集，因为 `2` 和 `3` 在两个区间中都被包含。

请你返回将 `ranges` 划分成两个组的 __总方案数__ 。由于答案可能很大，将它对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入：ranges = [[6,10],[5,15]]
输出：2
解释：
两个区间有交集，所以它们必须在同一个组内。
所以有两种方案：
```

- 将两个区间都放在第 1 个组中。
- 将两个区间都放在第 2 个组中。

示例 2：

```text
输入：ranges = [[1,3],[10,20],[2,5],[4,8]]
输出：4
解释：
区间 [1,3] 和 [2,5] 有交集，所以它们必须在同一个组中。
同理，区间 [2,5] 和 [4,8] 也有交集，所以它们也必须在同一个组中。
所以总共有 4 种分组方案：
```

- 所有区间都在第 1 组。
- 所有区间都在第 2 组。
- 区间 [1,3] ，[2,5] 和 [4,8] 在第 1 个组中，[10,20] 在第 2 个组中。
- 区间 [1,3] ，[2,5] 和 [4,8] 在第 2 个组中，[10,20] 在第 1 个组中。

__提示：__

- `1 <= ranges.length <= 10 ^ 5`
- `ranges[i].length == 2`
- `0 <= start_i <= end_i <= 10 ^ 9`

__思路:__

```text
合并区间
按照起点排序
排序之后每次检查当前区间的起点是否大于上一个区间的终点
如果大于, 则说明当前区间和上一个区间没有交集, 可以将当前区间放入另一个组中
统计区间的个数
可以任意放到两个组
所以返回 2 ^ result % MOD
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countWays(vector<vector<int>>& ranges) 
    {
        sort(ranges.begin(), ranges.end());
        int result = 1, m = -1, MOD = 1e9 + 7;
        for (const auto& p : ranges) 
        {
            if (p.front() > m) result = (result << 1) % MOD;
            m = max(m, p.back());
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countWays(int[][] ranges) {
        Arrays.sort(ranges, (a, b) -> a[0] - b[0]);
        int result = 1, m = -1, MOD = 1_000_000_007;
        for (int[] p : ranges) {
            if (p[0] > m) result = (result << 1) % MOD;
            m = Math.max(m, p[1]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countWays(self, ranges: List[List[int]]) -> int:
        ranges.sort(key=lambda p: p[0])
        result, m = 1, -1
        for l, r in ranges:
            if l > m:
                result <<= 1
            m = max(m, r)
        return result % (10 ** 9 + 7)
```
