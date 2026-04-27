# 1943 Describe the Painting 描述绘画结果

__Description:__

There is a long and thin painting that can be represented by a number line. The painting was painted with multiple overlapping segments where each segment was painted with a __unique__ color. You are given a 2D integer array `segments`, where `segments[i] = [starti, endi, colori]` represents the __half-closed segment__ `[starti, endi)` with `colori` as the color.

The colors in the overlapping segments of the painting were mixed when it was painted. When two or more colors mix, they form a new color that can be represented as a set of mixed colors.

- For example, if colors `2`, `4`, and `6` are mixed, then the resulting mixed color is `{2,4,6}`.

For the sake of simplicity, you should only output the sum of the elements in the set rather than the full set.

You want to __describe__ the painting with the __minimum__ number of non-overlapping __half-closed segments__ of these mixed colors. These segments can be represented by the 2D array `painting` where `painting[j] = [leftj, rightj, mixj]` describes a __half-closed segment__ `[leftj, rightj)` with the mixed color __sum__ of `mixj`.

- For example, the painting created with `segments = [[1,4,5],[1,7,7]]` can be described by `painting = [[1,4,12],[4,7,7]]` because:

- `[1,4)` is colored `{5,7}` (with a sum of `12`) from both the first and second segments.
- `[4,7)` is colored `{7}` from only the second segment.

Return _the 2D array_ `painting` _describing the finished painting (excluding any parts that are __not__ painted). You may return the segments in __any order___.

A __half-closed segment__ `[a, b)` is the section of the number line between points `a` and `b` __including__ point `a` and __not including__ point `b`.

__Example:__

Example 1:

![1943-1](https://assets.leetcode.com/uploads/2021/06/18/1.png)

```text
Input: segments = [[1,4,5],[4,7,7],[1,7,9]]
Output: [[1,4,14],[4,7,16]]
Explanation: The painting can be described as follows:
```

- [1,4) is colored {5,9} (with a sum of 14) from the first and third segments.
- [4,7) is colored {7,9} (with a sum of 16) from the second and third segments.

Example 2:

![1943-2](https://assets.leetcode.com/uploads/2021/06/18/2.png)

```text
Input: segments = [[1,7,9],[6,8,15],[8,10,7]]
Output: [[1,6,9],[6,7,24],[7,8,15],[8,10,7]]
Explanation: The painting can be described as follows:
```

- [1,6) is colored 9 from the first segment.
- [6,7) is colored {9,15} (with a sum of 24) from the first and second segments.
- [7,8) is colored 15 from the second segment.
- [8,10) is colored 7 from the third segment.

Example 3:

![1943-3](https://assets.leetcode.com/uploads/2021/07/04/c1.png)

```text
Input: segments = [[1,4,5],[1,4,7],[4,7,1],[4,7,11]]
Output: [[1,4,12],[4,7,12]]
Explanation: The painting can be described as follows:
```

- [1,4) is colored {5,7} (with a sum of 12) from the first and second segments.
- [4,7) is colored {1,11} (with a sum of 12) from the third and fourth segments.
Note that returning a single segment [1,7) is incorrect because the mixed color sets are different.

__Constraints:__

- `1 <= segments.length <= 2 * 10 ^ 4`
- `segments[i].length == 3`
- `1 <= starti < endi <= 10 ^ 5`
- `1 <= colori <= 10 ^ 9`
- Each `colori` is distinct.

__题目描述:__

给你一个细长的画，用数轴表示。这幅画由若干有重叠的线段表示，每个线段有 __独一无二__ 的颜色。给你二维整数数组 `segments` ，其中 `segments[i] = [starti, endi, colori]` 表示线段为 __半开区间__ `[starti, endi)` 且颜色为 `colori` 。

线段间重叠部分的颜色会被 混合 。如果有两种或者更多颜色混合时，它们会形成一种新的颜色，用一个 集合 表示这个混合颜色。

- 比方说，如果颜色 `2` ， `4` 和 `6` 被混合，那么结果颜色为 `{2,4,6}` 。

为了简化题目，你不需要输出整个集合，只需要用集合中所有元素的 和 来表示颜色集合。

你想要用 __最少数目__ 不重叠 __半开区间__ 来 _表示_ 这幅混合颜色的画。这些线段可以用二维数组 `painting` 表示，其中 `painting[j] = [leftj, rightj, mixj]` 表示一个 __半开区间__`[leftj, rightj)` 的颜色 __和__ 为 `mixj` 。

- 比方说，这幅画由 `segments = [[1,4,5],[1,7,7]]` 组成，那么它可以表示为 `painting = [[1,4,12],[4,7,7]]` ，因为:

- `[1,4)` 由颜色 `{5,7}` 组成（和为 `12`），分别来自第一个线段和第二个线段。
- `[4,7)` 由颜色 `{7}` 组成，来自第二个线段。

请你返回二维数组 `painting` ，它表示最终绘画的结果（__没有__ 被涂色的部分不出现在结果中）。你可以按 __任意顺序__ 返回最终数组的结果。

__半开区间__ `[a, b)` 是数轴上点 `a` 和点 `b` 之间的部分，__包含__ 点 `a` 且 __不包含__ 点 `b` 。

__示例:__

示例 1：

![1943-4](https://assets.leetcode.com/uploads/2021/06/18/1.png)

```text
输入：segments = [[1,4,5],[4,7,7],[1,7,9]]
输出：[[1,4,14],[4,7,16]]
解释：绘画结果可以表示为：
```

- [1,4) 颜色为 {5,9} （和为 14），分别来自第一和第二个线段。
- [4,7) 颜色为 {7,9} （和为 16），分别来自第二和第三个线段。

示例 2：

![1943-5](https://assets.leetcode.com/uploads/2021/06/18/2.png)

```text
输入：segments = [[1,7,9],[6,8,15],[8,10,7]]
输出：[[1,6,9],[6,7,24],[7,8,15],[8,10,7]]
解释：绘画结果可以以表示为：
```

- [1,6) 颜色为 9 ，来自第一个线段。
- [6,7) 颜色为 {9,15} （和为 24），来自第一和第二个线段。
- [7,8) 颜色为 15 ，来自第二个线段。
- [8,10) 颜色为 7 ，来自第三个线段。

示例 3：

![1943-6](https://assets.leetcode.com/uploads/2021/07/04/c1.png)

```text
输入：segments = [[1,4,5],[1,4,7],[4,7,1],[4,7,11]]
输出：[[1,4,12],[4,7,12]]
解释：绘画结果可以表示为：
- [1,4) 颜色为 {5,7} （和为 12），分别来自第一和第二个线段。
- [4,7) 颜色为 {1,11} （和为 12），分别来自第三和第四个线段。
注意，只返回一个单独的线段 [1,7) 是不正确的，因为混合颜色的集合不相同。
```

__提示：__

- `1 <= segments.length <= 2 * 10 ^ 4`
- `segments[i].length == 3`
- `1 <= starti < endi <= 10 ^ 5`
- `1 <= colori <= 10 ^ 9`
- 每种颜色 `colori` 互不相同。

__思路:__

```text
差分 ➕ 前缀和
先将每一段颜色做差分数组
求出差分数组的前缀和
最后将不为 0 的区间加入结果
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<long long>> splitPainting(vector<vector<int>>& segments) 
    {
        unordered_map<int, long long> color;
        for (const auto& segment : segments)
        {
            int l = segment[0], r = segment[1], c = segment[2];
            color[l] += c;
            color[r] -= c;
        }
        vector<pair<int, long long>> colors;
        for (const auto& [k, v] : color) colors.emplace_back(k, v);
        sort(colors.begin(), colors.end());
        int n = colors.size();
        for (int i = 1; i < n; ++i) colors[i].second += colors[i - 1].second;
        vector<vector<long long>> result;
        for (int i = 0; i < n - 1; ++i) if (colors[i].second) result.emplace_back(vector<long long> {colors[i].first, colors[i + 1].first, colors[i].second});
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Long>> splitPainting(int[][] segments) {
        int maxValue = 0;
        TreeSet<Integer> color = new TreeSet<>();
        for (int[] segment : segments) {
            maxValue = Math.max(segment[1], maxValue);
            color.add(segment[0]);
            color.add(segment[1]);
        }
        long[] diff = new long[maxValue + 1];
        for (int[] segment : segments) {
            diff[segment[0]] += segment[2];
            diff[segment[1]] -= segment[2];
        }
        for (int i = 1; i <= maxValue; i++) diff[i] += diff[i - 1];
        List<List<Long>> result = new ArrayList<>();
        while (color.size() > 1) {
            int l = color.pollFirst(), r = color.first();
            if (diff[l] != 0) result.add(Arrays.asList((long)l, (long)r, diff[l]));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def splitPainting(self, segments: List[List[int]]) -> List[List[int]]:
        color = defaultdict(int)
        for l, r, c in segments:
            color[l] += c
            color[r] -= c
        colors = sorted([[k, v] for k, v in color.items()])
        n, result = len(colors), []
        for i in range(1, n):
            colors[i][1] += colors[i - 1][1]
        for i in range(n - 1):
            if colors[i][1]:
                result.append([colors[i][0], colors[i + 1][0], colors[i][1]])
        return result
```
