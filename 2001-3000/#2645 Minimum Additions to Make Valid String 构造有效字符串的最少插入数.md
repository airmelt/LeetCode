# 2645 Minimum Additions to Make Valid String 构造有效字符串的最少插入数

__Description:__

Given a string `word` to which you can insert letters "a", "b" or "c" anywhere and any number of times, return _the minimum number of letters that must be inserted so that `word` becomes __valid__._

A string is called valid if it can be formed by concatenating the string "abc" several times.

__Example:__

Example 1:

```text
Input: word = "b"
Output: 2
Explanation: Insert the letter "a" right before "b", and the letter "c" right next to "b" to obtain the valid string "abc".
```

Example 2:

```text
Input: word = "aaa"
Output: 6
Explanation: Insert letters "b" and "c" next to each "a" to obtain the valid string "abcabcabc".
```

Example 3:

```text
Input: word = "abc"
Output: 0
Explanation: word is already valid. No modifications are needed.
```

__Constraints:__

- `1 <= word.length <= 50`
- `word` consists of letters "a", "b" and "c" only.

__题目描述:__

给你一个字符串 `word` ，你可以向其中任何位置插入 "a"、"b" 或 "c" 任意次，返回使 `word` __有效__ 需要插入的最少字母数。

如果字符串可以由 "abc" 串联多次得到，则认为该字符串 有效 。

__示例:__

示例 1：

```text
输入：word = "b"
输出：2
解释：在 "b" 之前插入 "a" ，在 "b" 之后插入 "c" 可以得到有效字符串 "abc" 。
```

示例 2：

```text
输入：word = "aaa"
输出：6
解释：在每个 "a" 之后依次插入 "b" 和 "c" 可以得到有效字符串 "abcabcabc" 。
```

示例 3：

```text
输入：word = "abc"
输出：0
解释：word 已经是有效字符串，不需要进行修改。
```

__提示：__

- `1 <= word.length <= 50`
- `word` 仅由字母 "a"、"b" 和 "c" 组成。

__思路:__

```text
动态规划
由于最终每一个字符都是 "abc" 按顺序排列
所以对于每一个字符
我们只需要考虑它与前一个字符的关系
当前是 a 下一个需要是 b, 如果是 c, 则需要插入 b
即 (c - a + 2) % 3
如果是 a 则需要插入 b 和 c
即 (a - a + 2) % 3
其余情况类似
最后因为开头必须是 a, 结尾必须是 c
还需要加上 word.front() - word.back() + 2
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int addMinimum(string word) 
    {
        int result = 0;
        for (int i = 1, n = word.length(); i < n; i++) result += (word[i] - word[i - 1] + 2) % 3;
        return result + word.front() - word.back() + 2;
    }
};
```

__Java__:

```Java
class Solution {
    public int addMinimum(String word) {
        int result = 0;
        for (int i = 1, n = word.length(); i < n; i++) result += (word.charAt(i) - word.charAt(i - 1) + 2) % 3;
        return result + word.charAt(0) - word.charAt(word.length() - 1) + 2;
    }
}
```

__Python__:

```Python
class Solution:
    def addMinimum(self, word: str) -> int:
        return sum((y - x + 2) % 3 for x, y in pairwise(map(ord, word))) + ord(word[0]) - ord(word[-1]) + 2
```
