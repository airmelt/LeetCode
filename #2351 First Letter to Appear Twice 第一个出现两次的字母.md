# 2351 First Letter to Appear Twice 第一个出现两次的字母

__Description:__

Given a string `s` consisting of lowercase English letters, return _the first letter to appear __twice___.

Note:

- A letter `a` appears twice before another letter `b` if the __second__ occurrence of `a` is before the __second__ occurrence of `b`.
- `s` will contain at least one letter that appears twice.

__Example:__

Example 1:

```text
Input: s = "abccbaacz"
Output: "c"
Explanation:
The letter 'a' appears on the indexes 0, 5 and 6.
The letter 'b' appears on the indexes 1 and 4.
The letter 'c' appears on the indexes 2, 3 and 7.
The letter 'z' appears on the index 8.
The letter 'c' is the first letter to appear twice, because out of all the letters the index of its second occurrence is the smallest.
```

Example 2:

```text
Input: s = "abcdd"
Output: "d"
Explanation:
The only letter that appears twice is 'd' so we return 'd'.
```

__Constraints:__

- `2 <= s.length <= 100`
- `s` consists of lowercase English letters.
- `s` has at least one repeated letter.

__题目描述:__

给你一个由小写英文字母组成的字符串 `s` ，请你找出并返回第一个出现 __两次__ 的字母。

注意：

- 如果 `a` 的 __第二次__ 出现比 `b` 的 __第二次__ 出现在字符串中的位置更靠前，则认为字母 `a` 在字母 `b` 之前出现两次。
- `s` 包含至少一个出现两次的字母。

__示例:__

示例 1：

```text
输入：s = "abccbaacz"
输出："c"
解释：
字母 'a' 在下标 0 、5 和 6 处出现。
字母 'b' 在下标 1 和 4 处出现。
字母 'c' 在下标 2 、3 和 7 处出现。
字母 'z' 在下标 8 处出现。
字母 'c' 是第一个出现两次的字母，因为在所有字母中，'c' 第二次出现的下标是最小的。
```

示例 2：

```text
输入：s = "abcdd"
输出："d"
解释：
只有字母 'd' 出现两次，所以返回 'd' 。
```

__提示：__

- `2 <= s.length <= 100`
- `s` 由小写英文字母组成
- `s` 包含至少一个重复字母

__思路:__

```text
1. 哈希表
用数组或者哈希表记录每个字母出现的次数
遍历字符串, 返回第一个出现两次的字母
时间复杂度为 O(N), 空间复杂度为 O(1), 只需要记录 26 个字母的出现次数
2. 位运算
用一个整数记录每个字母出现的次数
由于只有 26 个字母, 可以用一个二进制数的 26 位来记录每个字母的出现次数
cur |= (1 << (s[i] & 31)) 表示将第 s[i] 个字母对应的二进制位置为 1
遍历到第一次重复出现的字母, 就返回
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    char repeatedCharacter(string s) 
    {
        for (int i = 0, cur = 0, n = s.size(); i < n; i++) if (cur == (cur |= (1 << (s[i] & 31)))) return s[i];
        return '?';
    }
};
```

__Java__:

```Java
class Solution {
    public char repeatedCharacter(String s) {
        for (int i = 0, cur = 0, n = s.length(); i < n; i++) if (cur == (cur |= (1 << (s.charAt(i) & 31)))) return s.charAt(i);
        return '?';
    }
}
```

__Python__:

```Python
class Solution:
    def repeatedCharacter(self, s: str) -> str:
        return [c for i, c in enumerate(s) if c in s[:i]][0]
```
