# 2606 Find the Substring With Maximum Cost 找到最大开销的子字符串

__Description:__

You are given a string `s`, a string `chars` of __distinct__ characters and an integer array `vals` of the same length as `chars`.

The __cost of the substring__ is the sum of the values of each character in the substring. The cost of an empty string is considered `0`.

The value of the character is defined in the following way:

- If the character is not in the string `chars`, then its value is its corresponding position __(1-indexed)__ in the alphabet.

- Otherwise, assuming `i` is the index where the character occurs in the string `chars`, then its value is `vals[i]`.

- For example, the value of `'a'` is `1`, the value of `'b'` is `2`, and so on. The value of `'z'` is `26`.

Return _the maximum cost among all substrings of the string_ `s`.

__Example:__

Example 1:

```text
Input: s = "adaa", chars = "d", vals = [-1000]
Output: 2
Explanation: The value of the characters "a" and "d" is 1 and -1000 respectively.
The substring with the maximum cost is "aa" and its cost is 1 + 1 = 2.
It can be proven that 2 is the maximum cost.
```

Example 2:

```text
Input: s = "abc", chars = "abc", vals = [-1,-1,-1]
Output: 0
Explanation: The value of the characters "a", "b" and "c" is -1, -1, and -1 respectively.
The substring with the maximum cost is the empty substring "" and its cost is 0.
It can be proven that 0 is the maximum cost.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consist of lowercase English letters.
- `1 <= chars.length <= 26`
- `chars` consist of __distinct__ lowercase English letters.
- `vals.length == chars.length`
- `-1000 <= vals[i] <= 1000`

__题目描述:__

给你一个字符串 `s` ，一个字符 __互不相同__ 的字符串 `chars` 和一个长度与 `chars` 相同的整数数组 `vals` 。

__子字符串的开销__ 是一个子字符串中所有字符对应价值之和。空字符串的开销是 `0` 。

字符的价值 定义如下：

- 如果字符不在字符串 `chars` 中，那么它的价值是它在字母表中的位置（下标从 __1__ 开始）。

- 否则，如果这个字符在 `chars` 中的位置为 `i` ，那么它的价值就是 `vals[i]` 。

- 比方说， `'a'` 的价值为 `1` ， `'b'` 的价值为 `2` ，以此类推， `'z'` 的价值为 `26` 。

请你返回字符串 `s` 的所有子字符串中的最大开销。

__示例:__

示例 1：

```text
输入：s = "adaa", chars = "d", vals = [-1000]
输出：2
解释：字符 "a" 和 "d" 的价值分别为 1 和 -1000 。
最大开销子字符串是 "aa" ，它的开销为 1 + 1 = 2 。
2 是最大开销。
```

示例 2：

```text
输入：s = "abc", chars = "abc", vals = [-1,-1,-1]
输出：0
解释：字符 "a" ，"b" 和 "c" 的价值分别为 -1 ，-1 和 -1 。
最大开销子字符串是 "" ，它的开销为 0 。
0 是最大开销。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 只包含小写英文字母。
- `1 <= chars.length <= 26`
- `chars` 只包含小写英文字母，且 __互不相同__ 。
- `vals.length == chars.length`
- `-1000 <= vals[i] <= 1000`

__思路:__

```text
动态规划
先将小写字母都映射到对应的值
然后遍历字符串 s, 对于每个字符 c, 计算其对应的值
如果当前值 pre 小于 0, 则将其重置为 0
否则将其加到 pre 上
同时更新结果 result, 取 pre 和 result 的最大值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumCostSubstring(string s, string chars, vector<int>& vals) 
    {
        int values[26]{}, result = 0, pre = 0, n = vals.size();
        iota(values, values + 26, 1);
        for (int i = 0; i < n; i++) values[chars[i] - 'a'] = vals[i];
        for (const auto& c : s) result = max(result, pre = max(pre, 0) + values[c - 'a']);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumCostSubstring(String s, String chars, int[] vals) {
        int values[] = new int[26], result = 0, pre = 0, n = vals.length;
        for (int i = 0; i < 26; i++) values[i] = i + 1;
        for (int i = 0; i < n; i++) values[chars.charAt(i) - 'a'] = vals[i];
        for (char c : s.toCharArray()) result = Math.max(result, pre = Math.max(pre, 0) + values[c - 'a']);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumCostSubstring(self, s: str, chars: str, vals: List[int]) -> int:
        values, result, pre = dict(zip(ascii_lowercase, range(1, 27))) | dict(zip(chars, vals)), 0, 0
        for c in s:
            result = max(result, pre := max(pre, 0) + values[c])
        return result
```
