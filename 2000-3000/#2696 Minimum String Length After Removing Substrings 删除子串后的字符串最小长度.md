# 2696 Minimum String Length After Removing Substrings 删除子串后的字符串最小长度

__Description:__

You are given a string `s` consisting only of __uppercase__ English letters.

You can apply some operations to this string where, in one operation, you can remove __any__ occurrence of one of the substrings `"AB"` or `"CD"` from `s`.

Return the minimum possible length of the resulting string that you can obtain.

__Note__ that the string concatenates after removing the substring and could produce new `"AB"` or `"CD"` substrings.

__Example:__

Example 1:

```text
Input: s = "ABFCACDB"
Output: 2
Explanation: We can do the following operations:
```

- Remove the substring "ABFCACDB", so s = "FCACDB".
- Remove the substring "FCACDB", so s = "FCAB".
- Remove the substring "FCAB", so s = "FC".

So the resulting length of the string is 2.

It can be shown that it is the minimum length that we can obtain.

Example 2:

```text
Input: s = "ACBBD"
Output: 5
Explanation: We cannot do any operations on the string so the length remains the same.
```

__Constraints:__

- `1 <= s.length <= 100`
- `s` consists only of uppercase English letters.

__题目描述:__

给你一个仅由 __大写__ 英文字符组成的字符串 `s` 。

你可以对此字符串执行一些操作，在每一步操作中，你可以从 `s` 中删除 __任一个__ `"AB"` 或 `"CD"` 子字符串。

通过执行操作，删除所有 `"AB"` 和 `"CD"` 子串，返回可获得的最终字符串的 __最小__ 可能长度。

__注意__，删除子串后，重新连接出的字符串可能会产生新的 `"AB"` 或 `"CD"` 子串。

__示例:__

示例 1：

```text
输入：s = "ABFCACDB"
输出：2
解释：你可以执行下述操作：
```

- 从 "ABFCACDB" 中删除子串 "AB"，得到 s = "FCACDB" 。
- 从 "FCACDB" 中删除子串 "CD"，得到 s = "FCAB" 。
- 从 "FCAB" 中删除子串 "AB"，得到 s = "FC" 。

最终字符串的长度为 2 。

可以证明 2 是可获得的最小长度。

示例 2：

```text
输入：s = "ACBBD"
输出：5
解释：无法执行操作，字符串长度不变。
```

__提示：__

- `1 <= s.length <= 100`
- `s` 仅由大写英文字母组成

__思路:__

```text
1. 暴力法
每次检查 s 中是否还有 "AB" 或 "CD" 子串
如果有就替换成空串
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
2. 栈
将 "AB" 视为 "()"
将 "CD" 视为 "[]"
每次将栈顶元素和当前元素比较
如果满足条件就弹出
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minLength(string s) 
    {
        stack<char> st;
        for (const auto& c : s)
        {
            if (!st.empty() and (c == 'B' and st.top() == 'A' or c == 'D' and st.top() == 'C')) st.pop();
            else st.push(c);
        }
        return st.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int minLength(String s) {
        while (s.contains("AB") || s.contains("CD")) s = s.replace("AB", "").replace("CD", "");
        return s.length();
    }
}
```

__Python__:

```Python
class Solution:
    def minLength(self, s: str) -> int:
        return len(s) if "AB" not in s and "CD" not in s else self.minLength(s.replace("AB", "").replace("CD", ""))
```
