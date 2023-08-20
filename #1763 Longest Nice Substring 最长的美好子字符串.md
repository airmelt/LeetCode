# 1763 Longest Nice Substring 最长的美好子字符串

__Description:__

A string `s` is __nice__ if, for every letter of the alphabet that `s` contains, it appears __both__ in uppercase and lowercase. For example, `"abABB"` is nice because `'A'` and `'a'` appear, and `'B'` and `'b'` appear. However, `"abA"` is not because `'b'` appears, but `'B'` does not.

Given a string `s`, return _the longest __substring__ of `s` that is __nice__. If there are multiple, return the substring of the __earliest__ occurrence. If there are none, return an empty string_.

__Example:__

Example 1:

```text
Input: s = "YazaAay"
Output: "aAa"
Explanation: "aAa" is a nice string because 'A/a' is the only letter of the alphabet in s, and both 'A' and 'a' appear.
"aAa" is the longest nice substring.
```

Example 2:

```text
Input: s = "Bb"
Output: "Bb"
Explanation: "Bb" is a nice string because both 'B' and 'b' appear. The whole string is a substring.
```

Example 3:

```text
Input: s = "c"
Output: ""
Explanation: There are no nice substrings.
```

__Constraints:__

- `1 <= s.length <= 100`
- `s` consists of uppercase and lowercase English letters.

__题目描述:__

当一个字符串 `s` 包含的每一种字母的大写和小写形式 __同时__ 出现在 `s` 中，就称这个字符串 `s` 是 __美好__ 字符串。比方说， `"abABB"` 是美好字符串，因为 `'A'` 和 `'a'` 同时出现了，且 `'B'` 和 `'b'` 也同时出现了。然而， `"abA"` 不是美好字符串因为 `'b'` 出现了，而 `'B'` 没有出现。

给你一个字符串 `s` ，请你返回 `s` 最长的 __美好子字符串__ 。如果有多个答案，请你返回 __最早__ 出现的一个。如果不存在美好子字符串，请你返回一个空字符串。

__示例:__

示例 1：

```text
输入：s = "YazaAay"
输出："aAa"
解释："aAa" 是一个美好字符串，因为这个子串中仅含一种字母，其小写形式 'a' 和大写形式 'A' 也同时出现了。
"aAa" 是最长的美好子字符串。
```

示例 2：

```text
输入：s = "Bb"
输出："Bb"
解释："Bb" 是美好字符串，因为 'B' 和 'b' 都出现了。整个字符串也是原字符串的子字符串。
```

示例 3：

```text
输入：s = "c"
输出：""
解释：没有美好子字符串。
```

示例 4：

```text
输入：s = "dDzeE"
输出："dD"
解释："dD" 和 "eE" 都是最长美好子字符串。
由于有多个美好子字符串，返回 "dD" ，因为它出现得最早。
```

__提示：__

- `1 <= s.length <= 100`
- `s` 只包含大写和小写英文字母。

__思路:__

```text
1. 暴力法(Java)
检查每一个子串是否出现了相同的大写字母和小写字母
可以用二进制运算加快查找
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 分治法(C++)
如果整个字符串都包含字符的大写和小写, 即返回字符串本身
否则可以从未出现的字符为边界将字符分割
不能包括边界
最长的子串一定在被分割的段中
递归进行搜索
时间复杂度为 O(N), 空间复杂度为 O(1)
3. 滑动窗口法(Python)
维护一个窗口里的大小写字符的个数
左右边界 l, r
字符的数目 num
移动窗口的左右边界, 同时更新字符的种类 total 以及同时出现大小写的字符 count
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string longestNiceSubstring(string s) 
    {
        int pos = 0, len = 0;
        dfs(s, 0, s.size() - 1, pos, len);
        return s.substr(pos, len);
    }
private:
    void dfs(const string& s, int start, int end, int& maxPos, int& maxLen) 
    {
        if (start >= end) return;
        int lower = 0, upper = 0;
        for (int i = start; i <= end; i++) 
        {
            if (islower(s[i])) lower |= 1 << (s[i] - 'a');
            else upper |= 1 << (s[i] - 'A');
        }
        if (lower == upper) 
        {
            if (end - start + 1 > maxLen) 
            {
                maxPos = start;
                maxLen = end - start + 1;
            }
            return;
        } 
        int valid = lower & upper, pos = start;
        while (pos <= end) 
        {
            start = pos;
            while (pos <= end && valid & (1 << (tolower(s[pos]) - 'a'))) ++pos;
            dfs(s, start, pos - 1, maxPos, maxLen);
            ++pos;
        }
    }
};
```

__Java__:

```Java
class Solution {
    public String longestNiceSubstring(String s) {
        int n = s.length(), pos = 0, len = 0;
        for (int i = 0; i < n; i++) {
            int lower = 0, upper = 0;
            for (int j = i; j < n; j++) {
                if (Character.isLowerCase(s.charAt(j))) lower |= 1 << (s.charAt(j) - 'a');
                else upper |= 1 << (s.charAt(j) - 'A');
                if (lower == upper && j - i + 1 > len) {
                    pos = i;
                    len = j - i + 1;
                }
            }
        }
        return s.substring(pos, pos + len);
    }
}
```

__Python__:

```Python
class Solution:
    def longestNiceSubstring(self, s: str) -> str:
        def check(num: int) -> NoReturn:
            nonlocal maxPos, maxLen
            lower_count, upper_count, l, r, total, count = [0] * 26, [0] * 26, 0, 0, 0, 0
            while r < n:
                idx = ord(s[r].lower()) - ord('a')
                if s[r].islower():
                    lower_count[idx] += 1
                    if lower_count[idx] == 1 and upper_count[idx] > 0:
                        count += 1
                else:
                    upper_count[idx] += 1
                    if upper_count[idx] == 1 and lower_count[idx] > 0:
                        count += 1
                if lower_count[idx] + upper_count[idx] == 1:
                    total += 1
                while total > num:
                    idx = ord(s[l].lower()) - ord('a')
                    if lower_count[idx] + upper_count[idx] == 1:
                        total -= 1
                    if s[l].islower():
                        lower_count[idx] -= 1
                        if lower_count[idx] == 0 and upper_count[idx] > 0:
                            count -= 1
                    else:
                        upper_count[idx] -= 1
                        if upper_count[idx] == 0 and lower_count[idx] > 0:
                            count -= 1
                    l += 1
                if count == num and r - l + 1 > maxLen:
                    maxPos, maxLen = l, r - l + 1
                r += 1
        
        maxPos, maxLen, n, types = 0, 0, len(s), len(set(s.lower()))
        for i in range(1, types + 1):
            check(i)
        return s[maxPos: maxPos + maxLen]
```
