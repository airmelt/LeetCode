# 2315 Count Asterisks 统计星号

__Description:__

You are given a string `s`, where every __two__ consecutive vertical bars `'|'` are grouped into a __pair__. In other words, the 1 ^ st and 2 ^ nd `'|'` make a pair, the 3 ^ rd and 4 ^ th `'|'` make a pair, and so forth.

Return _the number of_ `'*'` _in_ `s`_, __excluding__ the_ `'*'` _between each pair of_ `'|'`.

__Note__ that each `'|'` will belong to __exactly__ one pair.

__Example:__

Example 1:

```text
Input: s = "l|*e*et|c**o|*de|"
Output: 2
Explanation: The considered characters are underlined: "l|*e*et|c**o|*de|".
The characters between the first and second '|' are excluded from the answer.
Also, the characters between the third and fourth '|' are excluded from the answer.
There are 2 asterisks considered. Therefore, we return 2.
```

Example 2:

```text
Input: s = "iamprogrammer"
Output: 0
Explanation: In this example, there are no asterisks in s. Therefore, we return 0.
```

Example 3:

```text
Input: s = "yo|uar|e**|b|e***au|tifu|l"
Output: 5
Explanation: The considered characters are underlined: "yo|uar|e**|b|e***au|tifu|l". There are 5 asterisks considered. Therefore, we return 5.
```

__Constraints:__

- `1 <= s.length <= 1000`
- `s` consists of lowercase English letters, vertical bars `'|'`, and asterisks `'*'`.
- `s` contains an __even__ number of vertical bars `'|'`.

__题目描述:__

给你一个字符串 `s` ，每 __两个__ 连续竖线 `'|'` 为 __一对__ 。换言之，第一个和第二个 `'|'` 为一对，第三个和第四个 `'|'` 为一对，以此类推。

请你返回 __不在__ 竖线对之间， `s` 中 `'*'` 的数目。

__注意__，每个竖线 `'|'` 都会 __恰好__ 属于一个对。

__示例:__

示例 1：

```text
输入：s = "l|*e*et|c**o|*de|"
输出：2
解释：不在竖线对之间的字符加粗加斜体后，得到字符串："l|*e*et|c**o|*de|" 。
第一和第二条竖线 '|' 之间的字符不计入答案。
同时，第三条和第四条竖线 '|' 之间的字符也不计入答案。
不在竖线对之间总共有 2 个星号，所以我们返回 2 。
```

示例 2：

```text
输入：s = "iamprogrammer"
输出：0
解释：在这个例子中，s 中没有星号。所以返回 0 。
```

示例 3：

```text
输入：s = "yo|uar|e**|b|e***au|tifu|l"
输出：5
解释：需要考虑的字符加粗加斜体后："yo|uar|e**|b|e***au|tifu|l" 。不在竖线对之间总共有 5 个星号。所以我们返回 5 。
```

__提示：__

- `1 <= s.length <= 1000`
- `s` 只包含小写英文字母，竖线 `'|'` 和星号 `'*'` 。
- `s` 包含 __偶数__ 个竖线 `'|'` 。

__思路:__

```text
1. 模拟
遍历字符串
记录 `|` 的个数
只有当 `|` 的个数为奇数时，才统计 `*` 的个数
也可以将字符串按 `|` 分割，统计偶数位置的字符串中 `*` 的个数
时间复杂度为 O(N), 空间复杂度为 O(1)
2. 正则表达式
使用正则表达式匹配 `|` 之间的字符串
regex = "\\|.*?\\|"
因为 `|` 是特殊字符，所以需要转义
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countAsterisks(string s) 
    {
        return count_if(s.cbegin(), s.cend(), [flag = 1](char c)mutable { return ((flag += (c == '|')) & 1) && c == '*'; });
    }
};
```

__Java__:

```Java
class Solution {
    public int countAsterisks(String s) {
        return s.replaceAll("\\|.*?\\||[^*]", "").length();
    }
}
```

__Python__:

```Python
class Solution:
    def countAsterisks(self, s: str) -> int:
        return sum(t.count('*') for t in s.split('|')[::2])
```
