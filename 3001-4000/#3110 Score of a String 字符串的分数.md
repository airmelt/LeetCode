# 3110 Score of a String 字符串的分数

__Description:__

You are given a string `s`. The __score__ of a string is defined as the sum of the absolute difference between the __ASCII__ values of adjacent characters.

Return the __score__ of `s`.

__Example:__

Example 1:

```text
Input: s = "hello"

Output: 13

Explanation:
```

The __ASCII__ values of the characters in `s` are: `'h' = 104`, `'e' = 101`, `'l' = 108`, `'o' = 111`. So, the score of `s` would be `|104 - 101| + |101 - 108| + |108 - 108| + |108 - 111| = 3 + 7 + 0 + 3 = 13`.

Example 2:

```text
Input: s = "zaz"

Output: 50

Explanation:
```

The __ASCII__ values of the characters in `s` are: `'z' = 122`, `'a' = 97`. So, the score of `s` would be `|122 - 97| + |97 - 122| = 25 + 25 = 50`.

__Constraints:__

- `2 <= s.length <= 100`
- `s` consists only of lowercase English letters.

__题目描述:__

给你一个字符串 `s` 。一个字符串的 __分数__ 定义为相邻字符 __ASCII__ 码差值绝对值的和。

请你返回 `s` 的 __分数__ 。

__示例:__

示例 1：

```text
输入：s = "hello"

输出：13

解释：
```

`s` 中字符的 __ASCII__ 码分别为: `'h' = 104` ， `'e' = 101` ， `'l' = 108` ， `'o' = 111` 。所以 `s` 的分数为 `|104 - 101| + |101 - 108| + |108 - 108| + |108 - 111| = 3 + 7 + 0 + 3 = 13` 。

示例 2：

```text
输入：s = "zaz"

输出：50

解释：
```

`s` 中字符的 __ASCII__ 码分别为: `'z' = 122` ， `'a' = 97` 。所以 `s` 的分数为 `|122 - 97| + |97 - 122| = 25 + 25 = 50` 。

__提示：__

- `2 <= s.length <= 100`
- `s` 只包含小写英文字母。

__思路:__

```text
模拟
计算相邻的字符差再求和
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int scoreOfString(string s) 
    {
        int result = 0, n = s.size();
        for (int i = 1; i < n; i++) result += abs(s[i] - s[i - 1]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int scoreOfString(String s) {
        return IntStream.range(1, s.length()).mapToObj(i -> Math.abs(s.charAt(i) - s.charAt(i - 1))).mapToInt(i -> i).sum();
    }
}
```

__Python__:

```Python
class Solution:
    def scoreOfString(self, s: str) -> int:
        return sum(abs(ord(x) - ord(y)) for x, y in pairwise(s))
```
