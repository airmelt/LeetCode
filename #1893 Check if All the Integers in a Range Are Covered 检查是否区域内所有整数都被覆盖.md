# 1893 Check if All the Integers in a Range Are Covered 检查是否区域内所有整数都被覆盖

__Description:__

You are given a 2D integer array `ranges` and two integers `left` and `right`. Each `ranges[i] = [starti, endi]` represents an __inclusive__ interval between `starti` and `endi`.

Return `true` _if each integer in the inclusive range_ `[left, right]` _is covered by __at least one__ interval in_ `ranges`. Return `false` _otherwise_.

An integer `x` is covered by an interval `ranges[i] = [starti, endi]` if `starti <= x <= endi`.

__Example:__

Example 1:

```text
Input: ranges = [[1,2],[3,4],[5,6]], left = 2, right = 5
Output: true
Explanation: Every integer between 2 and 5 is covered:
- 2 is covered by the first range.
- 3 and 4 are covered by the second range.
- 5 is covered by the third range.
```

Example 2:

```text
Input: ranges = [[1,10],[10,20]], left = 21, right = 21
Output: false
Explanation: 21 is not covered by any range.
```

__Constraints:__

- `1 <= ranges.length <= 50`
- `1 <= starti <= endi <= 50`
- `1 <= left <= right <= 50`

__题目描述:__

给你一个二维整数数组 `ranges` 和两个整数 `left` 和 `right` 。每个 `ranges[i] = [starti, endi]` 表示一个从 `starti` 到 `endi` 的 __闭区间__ 。

如果闭区间 `[left, right]` 内每个整数都被 `ranges` 中 __至少一个__ 区间覆盖，那么请你返回 `true` ，否则返回 `false` 。

已知区间 `ranges[i] = [starti, endi]` ，如果整数 `x` 满足 `starti <= x <= endi` ，那么我们称整数 `x` 被覆盖了。

__示例:__

示例 1：

```text
输入：ranges = [[1,2],[3,4],[5,6]], left = 2, right = 5
输出：true
解释：2 到 5 的每个整数都被覆盖了：
- 2 被第一个区间覆盖。
- 3 和 4 被第二个区间覆盖。
- 5 被第三个区间覆盖。
```

示例 2：

```text
输入：ranges = [[1,10],[10,20]], left = 21, right = 21
输出：false
解释：21 没有被任何一个区间覆盖。
```

__提示：__

- `1 <= ranges.length <= 50`
- `1 <= starti <= endi <= 50`
- `1 <= left <= right <= 50`

__思路:__

```text
1. 暴力法
对 [left, right] 中的每一个数字 i
检查是否有一个区间 [start, end] 使得 i 位于其中
时间复杂度为 O(MN), 空间复杂度为 O(1), 其中 N 为数组 ranges 的长度, M 为 [left, right] 中的数字个数
2. 差分数组 ➕ 前缀和
对于每一个区间 [start, end], 将 diff[start] 加一, diff[end + 1] 减一
对 diff 数组求前缀和
对于 [left, right] 中的每一个数字 i, 如果 diff[i] <= 0, 则说明该数字不被覆盖
时间复杂度为 O(N + M), 空间复杂度为 O(N), 其中 N 为数组 ranges 的长度, M 为 [left, right] 中的数字个数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isCovered(vector<vector<int>>& ranges, int left, int right) 
    {
        vector<int> diff(52);
        int n = ranges.size();
        for (int i = 0; i < n; i++)
        {
            diff[ranges[i][0]]++;
            diff[ranges[i][1] + 1]--;
        }
        for (int i = 1; i < 52; i++) diff[i] += diff[i - 1];
        for (int i = left; i <= right; i++) if (diff[i] <= 0) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isCovered(int[][] ranges, int left, int right) {
        int n = ranges.length, diff[] = new int[52];
        for (int i = 0; i < n; i++) {
            diff[ranges[i][0]]++;
            diff[ranges[i][1] + 1]--;
        }
        for (int i = 1; i < 52; i++) diff[i] += diff[i - 1];
        for (int i = left; i <= right; i++) if (diff[i] <= 0) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def isCovered(self, ranges: List[List[int]], left: int, right: int) -> bool:
        return all(any(start <= i <= end for start, end in ranges) for i in range(left, right + 1))
```
