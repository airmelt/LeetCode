# 2609 Find the Longest Balanced Substring of a Binary String 最长平衡子字符串

__Description:__

You are given a binary string `s` consisting only of zeroes and ones.

A substring of `s` is considered balanced if __all zeroes are before ones__ and the number of zeroes is equal to the number of ones inside the substring. Notice that the empty substring is considered a balanced substring.

Return _the length of the longest balanced substring of_ `s`.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "01000111"
Output: 6
Explanation: The longest balanced substring is "000111", which has length 6.
```

Example 2:

```text
Input: s = "00111"
Output: 4
Explanation: The longest balanced substring is "0011", which has length 4.
```

Example 3:

```text
Input: s = "111"
Output: 0
Explanation: There is no balanced substring except the empty substring, so the answer is 0.
```

__Constraints:__

- `1 <= s.length <= 50`
- `'0' <= s[i] <= '1'`

__题目描述:__

给你一个仅由 `0` 和 `1` 组成的二进制字符串 `s` 。

如果子字符串中 __所有的 __`0`__ 都在__ `1` __之前__ 且其中 `0` 的数量等于 `1` 的数量，则认为 `s` 的这个子字符串是平衡子字符串。请注意，空子字符串也视作平衡子字符串。

返回   `s` 中最长的平衡子字符串长度。

子字符串是字符串中的一个连续字符序列。

__示例:__

示例 1：

```text
输入：s = "01000111"
输出：6
解释：最长的平衡子字符串是 "000111" ，长度为 6 。
```

示例 2：

```text
输入：s = "00111"
输出：4
解释：最长的平衡子字符串是 "0011" ，长度为  4 。
```

示例 3：

```text
输入：s = "111"
输出：0
解释：除了空子字符串之外不存在其他平衡子字符串，所以答案为 0 。
```

__提示：__

- `1 <= s.length <= 50`
- `'0' <= s[i] <= '1'`

__思路:__

```text
模拟
记录当前字符的个数
如果遍历到结尾或者当前字符与下一个字符不相同
则判断当前字符是否为 '1'
如果是 '1'，则更新结果为当前字符个数和前一个字符个数的最小值乘以 2
然后将前一个字符个数更新为当前字符个数，当前字符个数归零
最后返回结果即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findTheLongestBalancedSubstring(string s) 
    {
        int result = 0, pre = 0, cur = 0, n = s.size();
        for (int i = 0; i < n; i++) 
        {
            ++cur;
            if (i == n - 1 or s[i] != s[i + 1]) 
            {
                if (s[i] == '1') result = max(result, min(pre, cur) << 1);
                pre = cur;
                cur = 0;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findTheLongestBalancedSubstring(String s) {
        int result = 0, pre = 0, cur = 0, n = s.length();
        for (int i = 0; i < n; i++) {
            ++cur;
            if (i == n - 1 || s.charAt(i) != s.charAt(i + 1)) {
                if (s.charAt(i) == '1') result = Math.max(result, Math.min(pre, cur) << 1);
                pre = cur;
                cur = 0;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findTheLongestBalancedSubstring(self, s: str) -> int:
        return max(i << 1 for i in range(26) if i * "0" + i * "1" in s)
```
