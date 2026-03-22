# 1910 Remove All Occurrences of a Substring 删除一个字符串中所有出现的给定子字符串

__Description:__

Given two strings `s` and `part`, perform the following operation on `s` until __all__ occurrences of the substring `part` are removed:

- Find the __leftmost__ occurrence of the substring `part` and __remove__ it from `s`.

Return `s` _after removing all occurrences of_ `part`.

A substring is a contiguous sequence of characters in a string.

__Example:__

Example 1:

```text
Input: s = "daabcbaabcbc", part = "abc"
Output: "dab"
Explanation: The following operations are done:
- s = "daabcbaabcbc", remove "abc" starting at index 2, so s = "dabaabcbc".
- s = "dabaabcbc", remove "abc" starting at index 4, so s = "dababc".
- s = "dababc", remove "abc" starting at index 3, so s = "dab".
Now s has no occurrences of "abc".
```

Example 2:

```text
Input: s = "axxxxyyyyb", part = "xy"
Output: "ab"
Explanation: The following operations are done:
- s = "axxxxyyyyb", remove "xy" starting at index 4 so s = "axxxyyyb".
- s = "axxxyyyb", remove "xy" starting at index 3 so s = "axxyyb".
- s = "axxyyb", remove "xy" starting at index 2 so s = "axyb".
- s = "axyb", remove "xy" starting at index 1 so s = "ab".
Now s has no occurrences of "xy".
```

__Constraints:__

- `1 <= s.length <= 1000`
- `1 <= part.length <= 1000`
- `s`​​​​​​ and `part` consists of lowercase English letters.

__题目描述:__

给你两个字符串 `s` 和 `part` ，请你对 `s` 反复执行以下操作直到 _所有_ 子字符串 `part` 都被删除:

- 找到 `s` 中 __最左边__ 的子字符串 `part` ，并将它从 `s` 中删除。

请你返回从 `s` 中删除所有 `part` 子字符串以后得到的剩余字符串。

一个 子字符串 是一个字符串中连续的字符序列。

__示例:__

示例 1：

```text
输入：s = "daabcbaabcbc", part = "abc"
输出："dab"
解释：以下操作按顺序执行：
- s = "daabcbaabcbc" ，删除下标从 2 开始的 "abc" ，得到 s = "dabaabcbc" 。
- s = "dabaabcbc" ，删除下标从 4 开始的 "abc" ，得到 s = "dababc" 。
- s = "dababc" ，删除下标从 3 开始的 "abc" ，得到 s = "dab" 。
此时 s 中不再含有子字符串 "abc" 。
```

示例 2：

```text
输入：s = "axxxxyyyyb", part = "xy"
输出："ab"
解释：以下操作按顺序执行：
- s = "axxxxyyyyb" ，删除下标从 4 开始的 "xy" ，得到 s = "axxxyyyb" 。
- s = "axxxyyyb" ，删除下标从 3 开始的 "xy" ，得到 s = "axxyyb" 。
- s = "axxyyb" ，删除下标从 2 开始的 "xy" ，得到 s = "axyb" 。
- s = "axyb" ，删除下标从 1 开始的 "xy" ，得到 s = "ab" 。
此时 s 中不再含有子字符串 "xy" 。
```

__提示：__

- `1 <= s.length <= 1000`
- `1 <= part.length <= 1000`
- `s`​​​​​​ 和 `part` 只包小写英文字母。

__思路:__

```text
模拟(KMP)
检查 part 是否在 s 中出现, 如果出现则删除, 重复此过程直到 part 不在 s 中出现
为了优化时间复杂度, 可以使用 KMP 算法
先计算 part 的 next 数组
然后遍历 s, 每次遍历到一个字符, 就更新 s 对应 的next 数组
时间复杂度为 O(N(N + M)), 空间复杂度为 O(N + M)
使用 KMP 算法可以将时间复杂度优化为 O(N + M)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string removeOccurrences(string s, string part) 
    {
        int m = part.size();
        vector<int> next1(m), next2 = {0};
        for (int i = 1, j = 0; i < m; i++) 
        {
            while (j and part[i] != part[j]) j = next1[j - 1];
            if (part[i] == part[j]) ++j;
            next1[i] = j;
        }
        string result;
        for (const auto& c : s) 
        {
            result.push_back(c);
            int j = next2.back();
            while (j and c != part[j]) j = next1[j - 1];
            if (c == part[j]) j++;
            next2.emplace_back(j);
            if (j == m) 
            {
                next2.erase(next2.end() - m, next2.end());
                result.erase(result.end() - m, result.end());
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String removeOccurrences(String s, String part) {
        StringBuffer sb = new StringBuffer(s);
        while (sb.indexOf(part) != -1) sb.delete(sb.indexOf(part), sb.indexOf(part) + part.length());
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def removeOccurrences(self, s: str, part: str) -> str:
        return s if part not in s else self.removeOccurrences(s.replace(part, '', 1), part)
```
