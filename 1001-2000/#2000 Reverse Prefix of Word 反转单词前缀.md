# 2000 Reverse Prefix of Word 反转单词前缀

__Description:__

Given a __0-indexed__ string `word` and a character `ch`, __reverse__ the segment of `word` that starts at index `0` and ends at the index of the __first occurrence__ of `ch` (__inclusive__). If the character `ch` does not exist in `word`, do nothing.

- For example, if `word = "abcdefd"` and `ch = "d"`, then you should __reverse__ the segment that starts at `0` and ends at `3` (__inclusive__). The resulting string will be `"dcbaefd"`.

Return the resulting string.

__Example:__

Example 1:

```text
Input: word = "abcdefd", ch = "d"
Output: "dcbaefd"
Explanation: The first occurrence of "d" is at index 3. 
Reverse the part of word from 0 to 3 (inclusive), the resulting string is "dcbaefd".
```

Example 2:

```text
Input: word = "xyxzxe", ch = "z"
Output: "zxyxxe"
Explanation: The first and only occurrence of "z" is at index 3.
Reverse the part of word from 0 to 3 (inclusive), the resulting string is "zxyxxe".
```

Example 3:

```text
Input: word = "abcd", ch = "z"
Output: "abcd"
Explanation: "z" does not exist in word.
You should not do any reverse operation, the resulting string is "abcd".
```

__Constraints:__

- `1 <= word.length <= 250`
- `word` consists of lowercase English letters.
- `ch` is a lowercase English letter.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `word` 和一个字符 `ch` 。找出 `ch` 第一次出现的下标 `i` ，__反转__ `word` 中从下标 `0` 开始、直到下标 `i` 结束（含下标 `i` ）的那段字符。如果 `word` 中不存在字符 `ch` ，则无需进行任何操作。

- 例如，如果 `word = "abcdefd"` 且 `ch = "d"` ，那么你应该 __反转__ 从下标 0 开始、直到下标 `3` 结束（含下标 `3` ）。结果字符串将会是 `"___dcba___efd"` 。

返回 结果字符串 。

__示例:__

示例 1：

```text
输入：word = "abcdefd", ch = "d"
输出："dcbaefd"
解释："d" 第一次出现在下标 3 。 
反转从下标 0 到下标 3（含下标 3）的这段字符，结果字符串是 "dcbaefd" 。
```

示例 2：

```text
输入：word = "xyxzxe", ch = "z"
输出："zxyxxe"
解释："z" 第一次也是唯一一次出现是在下标 3 。
反转从下标 0 到下标 3（含下标 3）的这段字符，结果字符串是 "zxyxxe" 。
```

示例 3：

```text
输入：word = "abcd", ch = "z"
输出："abcd"
解释："z" 不存在于 word 中。
无需执行反转操作，结果字符串是 "abcd" 。
```

__提示：__

- `1 <= word.length <= 250`
- `word` 由小写英文字母组成
- `ch` 是一个小写英文字母

__思路:__

```text
模拟
先查询 ch 的位置 idx, 查询不到为 -1
将 [0, idx + 1) 的字符串反转即可
可以用异或原地交换
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string reversePrefix(string word, char ch) 
    {
        reverse(word.begin(), word.begin() + word.find(ch) + 1);
        return word;
    }
};
```

__Java__:

```Java
class Solution {
    public String reversePrefix(String word, char ch) {
        return new StringBuilder(word.substring(0, word.indexOf(ch) + 1)).reverse().append(word.substring(word.indexOf(ch) + 1)).toString();
    }
}
```

__Python__:

```Python
class Solution:
    def reversePrefix(self, word: str, ch: str) -> str:
        return word[:word.find(ch) + 1][::-1] + word[word.find(ch) + 1:]
```
