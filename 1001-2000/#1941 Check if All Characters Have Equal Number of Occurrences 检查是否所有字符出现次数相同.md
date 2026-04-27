# 1941 Check if All Characters Have Equal Number of Occurrences 检查是否所有字符出现次数相同

__Description:__

Given a string `s`, return `true` _if_ `s` _is a __good__ string, or_ `false` _otherwise_.

A string `s` is __good__ if __all__ the characters that appear in `s` have the __same__ number of occurrences (i.e., the same frequency).

__Example:__

Example 1:

```text
Input: s = "abacbc"
Output: true
Explanation: The characters that appear in s are 'a', 'b', and 'c'. All characters occur 2 times in s.
```

Example 2:

```text
Input: s = "aaabb"
Output: false
Explanation: The characters that appear in s are 'a' and 'b'.
'a' occurs 3 times while 'b' occurs 2 times, which is not the same number of times.
```

__Constraints:__

- `1 <= s.length <= 1000`
- `s` consists of lowercase English letters.

__题目描述:__

给你一个字符串 `s` ，如果 `s` 是一个 __好__ 字符串，请你返回 `true` ，否则请返回 `false` 。

如果 `s` 中出现过的 __所有__ 字符的出现次数 __相同__ ，那么我们称字符串 `s` 是 __好__ 字符串。

__示例:__

示例 1：

```text
输入：s = "abacbc"
输出：true
解释：s 中出现过的字符为 'a'，'b' 和 'c' 。s 中所有字符均出现 2 次。
```

示例 2：

```text
输入：s = "aaabb"
输出：false
解释：s 中出现过的字符为 'a' 和 'b' 。
'a' 出现了 3 次，'b' 出现了 2 次，两者出现次数不同。
```

__提示：__

- `1 <= s.length <= 1000`
- `s` 只包含小写英文字母。

__思路:__

```text
模拟
记录所有字符的出现次数
要么出现次数为 0
要么出现次数是相同的
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool areOccurrencesEqual(string s) 
    {
        vector<int> nums(26);
        for (const auto& c : s) ++nums[c - 'a'];
        int pre = -1;
        for (const auto& num : nums)
        {
            if (num)
            {
                if (pre == -1) pre = num;
                else if (pre != num) return false;
            }
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean areOccurrencesEqual(String s) {
        return s.chars().boxed().collect(Collectors.groupingBy(Function.identity(), Collectors.counting())).values().stream().distinct().count() == 1;
    }
}
```

__Python__:

```Python
class Solution:
    def areOccurrencesEqual(self, s: str) -> bool:
        return len(set(Counter(s).values())) == 1
```
