# 1234 Replace the Substring for Balanced String 替换子串得到平衡字符串

__Description:__

You are given a string s of length n containing only four kinds of characters: 'Q', 'W', 'E', and 'R'.

A string is said to be balanced if each of its characters appears n / 4 times where n is the length of the string.

Return the minimum length of the substring that can be replaced with any other string of the same length to make s balanced. If s is already balanced, return 0.

__Example:__

Example 1:

Input: s = "QWER"
Output: 0
Explanation: s is already balanced.

Example 2:

Input: s = "QQWE"
Output: 1
Explanation: We need to replace a 'Q' to 'R', so that "RQWE" (or "QRWE") is balanced.

Example 3:

Input: s = "QQQW"
Output: 2
Explanation: We can replace the first "QQ" to "ER".

__Constraints:__

n == s.length
4 <= n <= 10^5
n is a multiple of 4.
s contains only 'Q', 'W', 'E', and 'R'.

__题目描述：__

有一个只含有 'Q', 'W', 'E', 'R' 四种字符，且长度为 n 的字符串。

假如在该字符串中，这四个字符都恰好出现 n/4 次，那么它就是一个「平衡字符串」。

给你一个这样的字符串 s，请通过「替换一个子串」的方式，使原字符串 s 变成一个「平衡字符串」。

你可以用和「待替换子串」长度相同的 任何 其他字符串来完成替换。

请返回待替换子串的最小可能长度。

如果原字符串自身就是一个平衡字符串，则返回 0。

__示例：__

示例 1：

输入：s = "QWER"
输出：0
解释：s 已经是平衡的了。
示例 2：

输入：s = "QQWE"
输出：1
解释：我们需要把一个 'Q' 替换成 'R'，这样得到的 "RQWE" (或 "QRWE") 是平衡的。

示例 3：

输入：s = "QQQW"
输出：2
解释：我们可以把前面的 "QQ" 替换成 "ER"。

示例 4：

输入：s = "QQQQ"
输出：3
解释：我们可以替换后 3 个 'Q'，使 s = "QWER"。

__提示：__

1 <= s.length <= 10^5
s.length 是 4 的倍数
s 中只含有 'Q', 'W', 'E', 'R' 四种字符

__思路：__

滑动窗口
先统计所有字符(QWER)的出现次数, 如果出现次数刚好都是字符串长度的 1/4, 说明不需要替换直接返回 0
然后使用滑动窗口, 窗口中的值要保证所有字符出现次数能够用来替换
也就是说减去滑动窗口中字符的出现次数, 剩下的字符出现次数需要都小于字符串长度的 1/4
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int balancedString(string s) 
    {
        int n = s.size(), target = (n >> 2), result = n, left = 0;
        vector<int> count(4);
        for (const auto& c : s) ++count[c == 'Q' ? 0 : (c == 'W' ? 1 : (c == 'E' ? 2 : 3))];
        if (count[0] == count[1] and count[2] == count[3] and count[1] == count[2]) return left;
        for (int right = 0; right < n; right++) 
        {
            --count[s[right] == 'Q' ? 0 : (s[right] == 'W' ? 1 : (s[right] == 'E' ? 2 : 3))];
            while (left <= right and count[0] <= target and count[1] <= target and count[2] <= target and count[3] <= target) 
            {
                result = min(result, right - left + 1);
                ++count[s[left] == 'Q' ? 0 : (s[left] == 'W' ? 1 : (s[left] == 'E' ? 2 : 3))];
                ++left;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int balancedString(String s) {
        int n = s.length(), target = (n >> 2), result = n, left = 0, count[] = new int[4];
        for (char c : s.toCharArray()) ++count[c == 'Q' ? 0 : (c == 'W' ? 1 : (c == 'E' ? 2 : 3))];
        if (count[0] == count[1] && count[2] == count[3] && count[1] == count[2]) return left;
        for (int right = 0; right < n; right++) {
            --count[s.charAt(right) == 'Q' ? 0 : (s.charAt(right) == 'W' ? 1 : (s.charAt(right) == 'E' ? 2 : 3))];
            while (left <= right && count[0] <= target && count[1] <= target && count[2] <= target && count[3] <= target) {
                result = Math.min(result, right - left + 1);
                ++count[s.charAt(left) == 'Q' ? 0 : (s.charAt(left) == 'W' ? 1 : (s.charAt(left) == 'E' ? 2 : 3))];
                ++left;
            }
        }
        return result;
    }   
}
```

__Python__:

```Python
class Solution:
    def balancedString(self, s: str) -> int:
        target, count, result, left = (n := len(s)) >> 2, Counter(s), n, 0
        if count['Q'] == target and count['W'] == target and count['E'] == target and count['R'] == target:
            return left
        for right in range(n):
            count[s[right]] -= 1
            while left <= right and count['Q'] <= target and count['W'] <= target and count['E'] <= target and count['R'] <= target:
                result = min(result, right - left + 1)
                count[s[left]] += 1
                left += 1
        return result
```
