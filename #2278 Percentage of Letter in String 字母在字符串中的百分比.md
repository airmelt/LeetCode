# 2278 Percentage of Letter in String 字母在字符串中的百分比

__Description:__

Given a string `s` and a character `letter`, return _the __percentage__ of characters in_ `s` _that equal_ `letter` ___rounded down__ to the nearest whole percent._

__Example:__

Example 1:

```text
Input: s = "foobar", letter = "o"
Output: 33
Explanation:
The percentage of characters in s that equal the letter 'o' is 2 / 6 * 100% = 33% when rounded down, so we return 33.
```

Example 2:

```text
Input: s = "jjjj", letter = "k"
Output: 0
Explanation:
The percentage of characters in s that equal the letter 'k' is 0%, so we return 0.
```

__Constraints:__

- `1 <= s.length <= 100`
- `s` consists of lowercase English letters.
- `letter` is a lowercase English letter.

__题目描述:__

给你一个字符串 `s` 和一个字符 `letter` ，返回在 `s` 中等于 `letter` 字符所占的 __百分比__ ，向下取整到最接近的百分比。

__示例:__

示例 1：

```text
输入：s = "foobar", letter = "o"
输出：33
解释：
等于字母 'o' 的字符在 s 中占到的百分比是 2 / 6 * 100% = 33% ，向下取整，所以返回 33 。
```

示例 2：

```text
输入：s = "jjjj", letter = "k"
输出：0
解释：
等于字母 'k' 的字符在 s 中占到的百分比是 0% ，所以返回 0 。
```

__提示：__

- `1 <= s.length <= 100`
- `s` 由小写英文字母组成
- `letter` 是一个小写英文字母

__思路:__

```text
模拟
统计 letter 在 s 出现的次数
先乘 100 再除以 s 的长度防止精度丢失
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int percentageLetter(string s, char letter) 
    {
        return count(s.begin(), s.end(), letter) * 100 / s.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int percentageLetter(String s, char letter) {
        return (int)s.chars().filter(c -> c == letter).count() * 100 / s.length();
    }
}
```

__Python__:

```Python
class Solution:
    def percentageLetter(self, s: str, letter: str) -> int:
        return s.count(letter) * 100 // len(s)
```
