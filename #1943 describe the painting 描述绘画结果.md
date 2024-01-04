# 1943 describe the painting 描述绘画结果

__Description:__

Given a string `s`, return `true` _if_ `s` _is a __good__ string, or_ `false` _otherwise_.

A string `s` is __good__ if __all__ the characters that appear in `s` have the __same__ number of occurrences (i.e., the same frequency).

__Example:__

Example 1:

```text
Input: s = "abacbc"
Output: true
Explanation: The characters that appear in s are 'a', 'b', and 'c'. All characters occur 2 times in s.
```

Example 2:

```text
Input: s = "aaabb"
Output: false
Explanation: The characters that appear in s are 'a' and 'b'.
'a' occurs 3 times while 'b' occurs 2 times, which is not the same number of times.
```

__Constraints:__

- `1 <= s.length <= 1000`
- `s` consists of lowercase English letters.

__题目描述:__

给你一个字符串 `s` ，如果 `s` 是一个 __好__ 字符串，请你返回 `true` ，否则请返回 `false` 。

如果 `s` 中出现过的 __所有__ 字符的出现次数 __相同__ ，那么我们称字符串 `s` 是 __好__ 字符串。

__示例:__

示例 1：

```text
输入：s = "abacbc"
输出：true
解释：s 中出现过的字符为 'a'，'b' 和 'c' 。s 中所有字符均出现 2 次。
```

示例 2：

```text
输入：s = "aaabb"
输出：false
解释：s 中出现过的字符为 'a' 和 'b' 。
'a' 出现了 3 次，'b' 出现了 2 次，两者出现次数不同。
```

__提示：__

- `1 <= s.length <= 1000`
- `s` 只包含小写英文字母。

__思路:__

```text
模拟
记录所有字符的出现次数
要么出现次数为 0
要么出现次数是相同的
时间复杂度为 O(N), 空间复杂度为 O(1)
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
