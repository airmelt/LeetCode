# 1358 Number of Substrings Containing All Three Characters 包含所有三种字符的子字符串数目

__Description:__

Given a string s consisting only of characters a, b and c.

Return the number of substrings containing at least one occurrence of all these characters a, b and c.

__Example:__

Example 1:

Input: s = "abcabc"
Output: 10
Explanation: The substrings containing at least one occurrence of the characters a, b and c are "abc", "abca", "abcab", "abcabc", "bca", "bcab", "bcabc", "cab", "cabc" and "abc" (again).

Example 2:

Input: s = "aaacb"
Output: 3
Explanation: The substrings containing at least one occurrence of the characters a, b and c are "aaacb", "aacb" and "acb".

Example 3:

Input: s = "abc"
Output: 1

__Constraints:__

3 <= s.length <= 5 x 10^4
s only consists of a, b or c characters.

__题目描述：__

给你一个字符串 s ，它只包含三种字符 a, b 和 c 。

请你返回 a，b 和 c 都 至少 出现过一次的子字符串数目。

__示例：__

示例 1：

输入：s = "abcabc"
输出：10
解释：包含 a，b 和 c 各至少一次的子字符串为 "abc", "abca", "abcab", "abcabc", "bca", "bcab", "bcabc", "cab", "cabc" 和 "abc" (相同字符串算多次)。

示例 2：

输入：s = "aaacb"
输出：3
解释：包含 a，b 和 c 各至少一次的子字符串为 "aaacb", "aacb" 和 "acb" 。

示例 3：

输入：s = "abc"
输出：1

__提示：__

3 <= s.length <= 5 x 10^4
s 只包含字符 a，b 和 c 。

__思路：__

滑动窗口
用一个数组记录出现过的字母的次数
双指针记录出现过的次数均大于 0 的起始位置
从该位置开始一直到末尾一定都至少含有 1 个 'abc' 字符
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int numberOfSubstrings(string s) 
    {
        int result = 0, n = s.size();
        vector<int> count(3);
        for (int left = 0, right = 0; right < n; right++)
        {
            ++count[s[right] - 'a'];
            while (count[0] > 0 and count[1] > 0 and count[2] > 0)
            {
                --count[s[left++] - 'a'];
                result += n - right;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfSubstrings(String s) {
        int result = 0, n = s.length(), count[] = new int[3];
        for (int left = 0, right = 0; right < n; right++) {
            ++count[s.charAt(right) - 'a'];
            while (count[0] > 0 && count[1] > 0 && count[2] > 0) {
                --count[s.charAt(left++) - 'a'];
                result += n - right;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfSubstrings(self, s: str) -> int:
        result, d, n = 0, {'a': -1, 'b': -1, 'c': -1}, len(s)
        for i in range(n):
            d[s[i]] = i
            if all(v != -1 for v in d.values()):
                result += min(d.values()) + 1
        return result
```
