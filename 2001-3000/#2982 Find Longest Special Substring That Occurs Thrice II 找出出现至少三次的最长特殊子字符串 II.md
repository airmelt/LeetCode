# 2982 Find Longest Special Substring That Occurs Thrice II 找出出现至少三次的最长特殊子字符串 II

__Description:__

You are given a string `s` that consists of lowercase English letters.

A string is called __special__ if it is made up of only a single character. For example, the string `"abc"` is not special, whereas the strings `"ddd"`, `"zz"`, and `"f"` are special.

Return _the length of the __longest special substring__ of_ `s` _which occurs __at least thrice___, _or_ `-1` _if no special substring occurs at least thrice_.

A substring is a contiguous non-empty sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "aaaa"
Output: 2
Explanation: The longest special substring which occurs thrice is "aa": substrings "aaaa", "aaaa", and "aaaa".
It can be shown that the maximum length achievable is 2.
```

Example 2:

```text
Input: s = "abcdef"
Output: -1
Explanation: There exists no special substring which occurs at least thrice. Hence return -1.
```

Example 3:

```text
Input: s = "abcaba"
Output: 1
Explanation: The longest special substring which occurs thrice is "a": substrings "abcaba", "abcaba", and "abcaba".
It can be shown that the maximum length achievable is 1.
```

__Constraints:__

- `3 <= s.length <= 5 * 10 ^ 5`
- `s` consists of only lowercase English letters.

__题目描述:__

给你一个仅由小写英文字母组成的字符串 `s` 。

如果一个字符串仅由单一字符组成，那么它被称为 __特殊__ 字符串。例如，字符串 `"abc"` 不是特殊字符串，而字符串 `"ddd"`、 `"zz"` 和 `"f"` 是特殊字符串。

返回在 `s` 中出现 __至少三次__ 的 __最长特殊子字符串__ 的长度，如果不存在出现至少三次的特殊子字符串，则返回 `-1` 。

子字符串 是字符串中的一个连续 非空 字符序列。

__示例:__

示例 1：

```text
输入：s = "aaaa"
输出：2
解释：出现三次的最长特殊子字符串是 "aa" ：子字符串 "aaaa"、"aaaa" 和 "aaaa"。
可以证明最大长度是 2 。
```

示例 2：

```text
输入：s = "abcdef"
输出：-1
解释：不存在出现至少三次的特殊子字符串。因此返回 -1 。
```

示例 3：

```text
输入：s = "abcaba"
输出：1
解释：出现三次的最长特殊子字符串是 "a" ：子字符串 "abcaba"、"abcaba" 和 "abcaba"。
可以证明最大长度是 1 。
```

__提示：__

- `3 <= s.length <= 5 * 10 ^ 5`
- `s` 仅由小写英文字母组成。

__思路:__

```text
模拟
按照不同的字符串分组, 记录连续字符的长度
比如 "aada" 可以分成 a: [2, 1], d: [1]
将长度数组从大到小排序
比如 a: [2, 1]
为了避免讨论边界情况, 可以塞两个空值, a: [2, 1, 0, 0]
设这个长度数组为 v
分别可以从最长的字符串里取 3 个长度为 v[0] - 2 的字符串
取 3 个最短的 v[2] 的字符串
还可以从最长的取 2 个 v[1] 以及 v[1] 自身
或者取 3 个 v[0] - 1
后两种情况取决于 min(v[0] - 1, v[1])
综上取 max(v[0] - 2, min(v[0] - 1, v[1]), v[2])
即为当前字符的最大值
取得全局的最大值 result
若 result 大于 0 说明能取到有意义的值, 返回 result
否则返回 -1
时间复杂度为 O(N), 空间复杂度为 O(N), 只需要用堆维护最大的三个值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumLength(string s) 
    {
        vector<vector<int>> groups(26, vector<int>(2));
        int cur = 0, n = s.length(), result = 0;
        for (int i = 0; i < n; i++) 
        {
            ++cur;
            if (i + 1 == n or s[i] != s[i + 1]) 
            {
                groups[s[i] - 'a'].emplace_back(cur);
                cur = 0;
            }
        }
        for (auto& v : groups) 
        {
            if (v.size() > 2)
            {
                sort(v.begin(), v.end(), greater());
                result = max({ result, v[0] - 2, min(v[0] - 1, v[1]), v[2] });
            }
        }
        return result ? result : -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumLength(String s) {
        List<Integer>[] groups = new ArrayList[26];
        Arrays.setAll(groups, i -> new ArrayList<>(){{ add(0); add(0); }});
        int cur = 0, result = 0, n = s.length();
        for (int i = 0; i < n; i++) {
            ++cur;
            if (i + 1 == n || s.charAt(i) != s.charAt(i + 1)) {
                groups[s.charAt(i) - 'a'].add(cur);
                cur = 0;
            }
        }
        for (var v : groups) {
            if (v.size() > 2) {
                v.sort(Collections.reverseOrder());
                result = Math.max(result, Math.max(v.get(0) - 2, Math.max(Math.min(v.get(0) - 1, v.get(1)), v.get(2))));
            }
        }

        return result > 0 ? result : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumLength(self, s: str) -> int:
        groups, cur, n, result = defaultdict(list), 0, len(s), 0
        for i, c in enumerate(s):
            cur += 1
            if i + 1 == n or c != s[i + 1]:
                groups[c].append(cur)
                cur = 0
        return result if (result := max(max((a := sorted(v + [0, 0]))[-1] - 2, min(a[-1] - 1, a[-2]), a[-3]) for v in groups.values())) else -1

```
