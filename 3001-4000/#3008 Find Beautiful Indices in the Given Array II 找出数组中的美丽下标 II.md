# 3008 Find Beautiful Indices in the Given Array II 找出数组中的美丽下标 II

__Description:__

You are given a __0-indexed__ string `s`, a string `a`, a string `b`, and an integer `k`.

An index `i` is __beautiful__ if:

- `0 <= i <= s.length - a.length`
- `s[i..(i + a.length - 1)] == a`

There exists an index `j` such that:

- `0 <= j <= s.length - b.length`
- `s[j..(j + b.length - 1)] == b`
- `|j - i| <= k`

Return the array that contains beautiful indices in sorted order from smallest to largest.

__Example:__

Example 1:

```text
Input: s = "isawsquirrelnearmysquirrelhouseohmy", a = "my", b = "squirrel", k = 15
Output: [16,33]
Explanation: There are 2 beautiful indices: [16,33].
```

- The index 16 is beautiful as s[16..17] == "my" and there exists an index 4 with s[4..11] == "squirrel" and |16 - 4| <= 15.
- The index 33 is beautiful as s[33..34] == "my" and there exists an index 18 with s[18..25] == "squirrel" and |33 - 18| <= 15.

Thus we return [16,33] as the result.

Example 2:

```text
Input: s = "abcd", a = "a", b = "a", k = 4
Output: [0]
Explanation: There is 1 beautiful index: [0].
```

- The index 0 is beautiful as s[0..0] == "a" and there exists an index 0 with s[0..0] == "a" and |0 - 0| <= 4.

Thus we return [0] as the result.

__Constraints:__

- `1 <= k <= s.length <= 5 * 10 ^ 5`
- `1 <= a.length, b.length <= 5 * 10 ^ 5`
- `s`, `a`, and `b` contain only lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` 、字符串 `a` 、字符串 `b` 和一个整数 `k` 。

如果下标 `i` 满足以下条件，则认为它是一个 __美丽下标__ :

- `0 <= i <= s.length - a.length`
- `s[i..(i + a.length - 1)] == a`

- 存在下标 `j` 使得:

- `0 <= j <= s.length - b.length`
- `s[j..(j + b.length - 1)] == b`
- `|j - i| <= k`

以数组形式按 从小到大排序 返回美丽下标。

__示例:__

示例 1：

```text
输入：s = "isawsquirrelnearmysquirrelhouseohmy", a = "my", b = "squirrel", k = 15
输出：[16,33]
解释：存在 2 个美丽下标：[16,33]。
```

- 下标 16 是美丽下标，因为 s[16..17] == "my" ，且存在下标 4 ，满足 s[4..11] == "squirrel" 且 |16 - 4| <= 15 。
- 下标 33 是美丽下标，因为 s[33..34] == "my" ，且存在下标 18 ，满足 s[18..25] == "squirrel" 且 |33 - 18| <= 15 。

因此返回 [16,33] 作为结果。

示例 2：

```text
输入：s = "abcd", a = "a", b = "a", k = 4
输出：[0]
解释：存在 1 个美丽下标：[0]。
```

- 下标 0 是美丽下标，因为 s[0..0] == "a" ，且存在下标 0 ，满足 s[0..0] == "a" 且 |0 - 0| <= 4 。

因此返回 [0] 作为结果。

__提示：__

- `1 <= k <= s.length <= 5 * 10 ^ 5`
- `1 <= a.length, b.length <= 5 * 10 ^ 5`
- `s`、 `a`、和 `b` 只包含小写英文字母。

__思路:__

```text
KMP ➕ 双指针
使用 KMP 分别求出 a 和 b 在 s 中的所有位置
记为 pos_a, pos_b
由于这两个数组有序
用双指针找到两个数组距离小于 k 的所有下标组合
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> beautifulIndices(string s, string a, string b, int k) 
    {   
        auto kmp = [](const auto& text, const auto& pattern) -> vector<int>
        {
            int m = pattern.size(), i = 1, j = 0, n = text.size();
            vector<int> nxt(m), result;
            for (char c = i < m ? pattern[i] : 'a'; i < m; i++, c = i < m ? pattern[i] : c) 
            {
                while (j and pattern[j] != c) j = nxt[j - 1];
                if (pattern[j] == c) ++j;
                nxt[i] = j;
            }
            for (i = 0, j = 0; i < n; i++) 
            {
                while (j and pattern[j] != text[i]) j = nxt[j - 1];
                if (pattern[j] == text[i]) ++j;
                if (j == m) 
                {
                    result.emplace_back(i - m + 1);
                    j = nxt[j - 1];
                }
            }
            return result;
        };
        vector<int> pos_a = kmp(s, a), pos_b = kmp(s, b), result;
        int j = 0, m = pos_b.size();
        for (const auto& i : pos_a) 
        {
            while (j < m and pos_b[j] < i - k) ++j;
            if (j < m and pos_b[j] <= i + k) result.emplace_back(i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> beautifulIndices(String s, String a, String b, int k) {
        List<Integer> posA = kmp(s.toCharArray(), a.toCharArray()), posB = kmp(s.toCharArray(), b.toCharArray()), result = new ArrayList<>();
        int j = 0, m = posB.size();
        for (int i : posA) {
            while (j < m && posB.get(j) < i - k) ++j;
            if (j < m && posB.get(j) <= i + k) result.add(i);
        }
        return result;
    }

    private List<Integer> kmp(char[] text, char[] pattern) {
        int m = pattern.length, nxt[] = new int[m], i = 1, j = 0, n = text.length;
        for (char c = i < m ? pattern[i] : 'a'; i < m; i++, c = i < m ? pattern[i] : c) {
            while (j > 0 && pattern[j] != c) j = nxt[j - 1];
            if (pattern[j] == c) ++j;
            nxt[i] = j;
        }
        List<Integer> result = new ArrayList<>();
        for (i = 0, j = 0; i < n; i++) {
            while (j > 0 && pattern[j] != text[i]) j = nxt[j - 1];
            if (pattern[j] == text[i]) ++j;
            if (j == m) {
                result.add(i - m + 1);
                j = nxt[j - 1];
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def beautifulIndices(self, s: str, a: str, b: str, k: int) -> List[int]:
        def kmp(text: str, pattern: str) -> List[int]:
            nxt, j = [0] * (m := len(pattern)), 0
            for i in range(1, m):
                c = pattern[i]
                while j and pattern[j] != c:
                    j = nxt[j - 1]
                if pattern[j] == c:
                    j += 1
                nxt[i] = j
            result, j = [], 0
            for i, c in enumerate(text):
                while j and pattern[j] != c:
                    j = nxt[j - 1]
                if pattern[j] == c:
                    j += 1
                if j == m:
                    result.append(i - m + 1)
                    j = nxt[j - 1]
            return result
        pos_a, m, result, j = kmp(s, a), len(pos_b := kmp(s, b)), [], 0
        for i in pos_a:
            while j < m and pos_b[j] < i - k:
                j += 1
            if j < m and pos_b[j] <= i + k:
                result.append(i)
        return result
```
