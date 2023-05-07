# 1624 Largest Substring Between Two Equal Characters 两个相同字符之间的最长子字符串

__Description:__

Given a string `s`, return _the length of the longest substring between two equal characters, excluding the two characters._ If there is no such substring return `-1`.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "aa"
Output: 0
Explanation: The optimal substring here is an empty substring between the two `'a's`.
```

Example 2:

```text
Input: s = "abca"
Output: 2
Explanation: The optimal substring here is "bc".
```

Example 3:

```text
Input: s = "cbzxy"
Output: -1
Explanation: There are no characters that appear twice in s.
```

__Constraints:__

- `1 <= s.length <= 300`
- `s` contains only lowercase English letters.

__题目描述:__

给你一个字符串 `s`，请你返回 __两个相同字符之间的最长子字符串的长度__ ，计算长度时不含这两个字符。如果不存在这样的子字符串，返回 `-1` 。

子字符串 是字符串中的一个连续字符序列。

__示例:__

示例 1：

```text
输入：s = "aa"
输出：0
解释：最优的子字符串是两个 'a' 之间的空子字符串。
```

示例 2：

```text
输入：s = "abca"
输出：2
解释：最优的子字符串是 "bc" 。
```

示例 3：

```text
输入：s = "cbzxy"
输出：-1
解释：s 中不存在出现出现两次的字符，所以返回 -1 。
```

示例 4：

```text
输入：s = "cabbac"
输出：4
解释：最优的子字符串是 "abba" ，其他的非最优解包括 "bb" 和 "" 。
```

__提示：__

- `1 <= s.length <= 300`
- `s` 只含小写英文字母

__思路:__

```text
模拟
记录下每个字符第一次出现的位置
找到每一个相同字符出现的位置并更新最大值
时间复杂度为 O(N), 空间复杂度为 O(1), 只需要 26 个位置信息为常数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxLengthBetweenEqualCharacters(string s) 
    {
        vector<int> pos(26, -1);
        int result = -1, n = s.size();
        for (int i = 0; i < n; i++) 
        {
            if (pos[s[i] - 'a'] < 0) pos[s[i] - 'a'] = i;
            else result = max(result, i - pos[s[i] - 'a'] - 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxLengthBetweenEqualCharacters(String s) {
        int result = -1, n = s.length();
        for (int i = 0; i < n; i++) result = Math.max(result, s.lastIndexOf(s.charAt(i)) - i - 1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxLengthBetweenEqualCharacters(self, s: str) -> int:
        return max([-1] + [s.rfind(i) - s.find(i) - 1 for i in set(s)])
```
