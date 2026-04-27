# 2301 Match Substring After Replacement 替换字符后匹配

__Description:__

You are given two strings `s` and `sub`. You are also given a 2D character array `mappings` where `mappings[i] = [old_i, new_i]` indicates that you may perform the following operation __any__ number of times:

- __Replace__ a character `old_i` of `sub` with `new_i`.

Each character in `sub` __cannot__ be replaced more than once.

Return `true` _if it is possible to make_ `sub` _a substring of_ `s` _by replacing zero or more characters according to_ `mappings`. Otherwise, return `false`.

A substring is a contiguous non-empty sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "fool3e7bar", sub = "leet", mappings = [["e","3"],["t","7"],["t","8"]]
Output: true
Explanation: Replace the first 'e' in sub with '3' and 't' in sub with '7'.
Now sub = "l3e7" is a substring of s, so we return true.
```

Example 2:

```text
Input: s = "fooleetbar", sub = "f00l", mappings = [["o","0"]]
Output: false
Explanation: The string "f00l" is not a substring of s and no replacements can be made.
Note that we cannot replace '0' with 'o'.
```

Example 3:

```text
Input: s = "Fool33tbaR", sub = "leetd", mappings = [["e","3"],["t","7"],["t","8"],["d","b"],["p","b"]]
Output: true
Explanation: Replace the first and second 'e' in sub with '3' and 'd' in sub with 'b'.
Now sub = "l33tb" is a substring of s, so we return true.
```

__Constraints:__

- `1 <= sub.length <= s.length <= 5000`
- `0 <= mappings.length <= 1000`
- `mappings[i].length == 2`
- `old_i != new_i`
- `s` and `sub` consist of uppercase and lowercase English letters and digits.
- `old_i` and `new_i` are either uppercase or lowercase English letters or digits.

__题目描述:__

给你两个字符串 `s` 和 `sub` 。同时给你一个二维字符数组 `mappings` ，其中 `mappings[i] = [old_i, new_i]` 表示你可以将 `sub` 中任意数目的 `old_i` 字符替换为 `new_i` 。 `sub` 中每个字符 _不能_ 被替换超过一次。

如果使用 `mappings` 替换 0 个或者若干个字符，可以将 `sub` 变成 `s` 的一个子字符串，请你返回 `true`，否则返回 `false` 。

一个 子字符串 是字符串中连续非空的字符序列。

__示例:__

示例 1：

```text
输入：s = "fool3e7bar", sub = "leet", mappings = [["e","3"],["t","7"],["t","8"]]
输出：true
解释：将 sub 中第一个 'e' 用 '3' 替换，将 't' 用 '7' 替换。
现在 sub = "l3e7" ，它是 s 的子字符串，所以我们返回 true 。
```

示例 2：

```text
输入：s = "fooleetbar", sub = "f00l", mappings = [["o","0"]]
输出：false
解释：字符串 "f00l" 不是 s 的子串且没有可以进行的修改。
注意我们不能用 'o' 替换 '0' 。
```

示例 3：

```text
输入：s = "Fool33tbaR", sub = "leetd", mappings = [["e","3"],["t","7"],["t","8"],["d","b"],["p","b"]]
输出：true
解释：将 sub 里第一个和第二个 'e' 用 '3' 替换，用 'b' 替换 sub 里的 'd' 。
得到 sub = "l33tb" ，它是 s 的子字符串，所以我们返回 true 。
```

__提示：__

- `1 <= sub.length <= s.length <= 5000`
- `0 <= mappings.length <= 1000`
- `mappings[i].length == 2`
- `old_i != new_i`
- `s` 和 `sub` 只包含大写和小写英文字母和数字。
- `old_i` 和 `new_i` 是大写、小写字母或者是个数字。

__思路:__

```text
哈希表
遍历 mappings, 将 mappings 中的映射关系存储在哈希表中
遍历 s, 对于 s 中的每一个字符, 从 s 的当前位置开始, 与 sub 进行匹配
如果 s 中的字符与 sub 中的字符相等, 或者 s 中的字符与 sub 中的字符在 mappings 中有映射关系, 则继续匹配
直到匹配完成或者匹配失败继续下一个位置的匹配
时间复杂度为 O(MN), 空间复杂度为 O(P), 其中 M 为 s 的长度, N 为 sub 的长度, P 为 mappings 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool matchReplacement(string s, string sub, vector<vector<char>>& mappings) 
    {
        vector<vector<bool>> mp(128, vector<bool>(128));
        for (const auto& mapping : mappings) mp[mapping.front()][mapping.back()] = true;
        for (int m = s.length(), n = sub.length(), i = n; i <= m; i++) 
        {
            bool flag = true;
            for (int start = i - n, j = start; j < i; j++) 
            {
                if (s[j] != sub[j - start] and !mp[sub[j - start]][s[j]])
                {
                    flag = false;
                    break;
                }
            }
            if (flag) return flag;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean matchReplacement(String s, String sub, char[][] mappings) {
        boolean[][] mp = new boolean[128][128];
        for (char[] mapping : mappings) mp[mapping[0]][mapping[1]] = true;
        for (int m = s.length(), n = sub.length(), i = n; i <= m; i++) {
            boolean flag = true;
            for (int start = i - n, j = start; j < i; j++) {
                if (s.charAt(j) != sub.charAt(j - start) && !mp[sub.charAt(j - start)][s.charAt(j)]) {
                    flag = false;
                    break;
                }
            }
            if (flag) return flag;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def matchReplacement(self, s: str, sub: str, mappings: List[List[str]]) -> bool:
        return (m := len(s)) and (n := len(sub)) and sub in s or (bool(st := {(x, y) for x, y in mappings}) and any(all(x == y or (x, y) in st for x, y in zip(sub, s[i - len(sub): i])) for i in range(n, m + 1)))
```
