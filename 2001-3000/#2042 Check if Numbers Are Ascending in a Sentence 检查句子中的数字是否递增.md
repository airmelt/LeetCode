# 2042 Check if Numbers Are Ascending in a Sentence 检查句子中的数字是否递增

__Description:__

A sentence is a list of __tokens__ separated by a __single__ space with no leading or trailing spaces. Every token is either a __positive number__ consisting of digits `0-9` with no leading zeros, or a __word__ consisting of lowercase English letters.

- For example, `"a puppy has 2 eyes 4 legs"` is a sentence with seven tokens: `"2"` and `"4"` are numbers and the other tokens such as `"puppy"` are words.

Given a string `s` representing a sentence, you need to check if __all__ the numbers in `s` are __strictly increasing__ from left to right (i.e., other than the last number, __each__ number is __strictly smaller__ than the number on its __right__ in `s`).

Return `true` _if so, or_ `false` _otherwise_.

__Example:__

Example 1:

![2042-1](https://assets.leetcode.com/uploads/2021/09/30/example1.png)

```text
Input: s = "1 box has 3 blue 4 red 6 green and 12 yellow marbles"
Output: true
Explanation: The numbers in s are: 1, 3, 4, 6, 12.
They are strictly increasing from left to right: 1 < 3 < 4 < 6 < 12.
```

Example 2:

```text
Input: s = "hello world 5 x 5"
Output: false
Explanation: The numbers in s are: 5, 5. They are not strictly increasing.
```

Example 3:

![2042-2](https://assets.leetcode.com/uploads/2021/09/30/example3.png)

```text
Input: s = "sunset is at 7 51 pm overnight lows will be in the low 50 and 60 s"
Output: false
Explanation: The numbers in s are: 7, 51, 50, 60. They are not strictly increasing.
```

__Constraints:__

- `3 <= s.length <= 200`
- `s` consists of lowercase English letters, spaces, and digits from `0` to `9`, inclusive.
- The number of tokens in `s` is between `2` and `100`, inclusive.
- The tokens in `s` are separated by a single space.
- There are at least __two__ numbers in `s`.
- Each number in `s` is a __positive__ number __less__ than `100`, with no leading zeros.
- `s` contains no leading or trailing spaces.

__题目描述:__

句子是由若干 __token__ 组成的一个列表，__token__ 间用 __单个__ 空格分隔，句子没有前导或尾随空格。每个 token 要么是一个由数字 `0-9` 组成的不含前导零的 __正整数__ ，要么是一个由小写英文字母组成的 __单词__ 。

- 示例， `"a puppy has 2 eyes 4 legs"` 是一个由 7 个 token 组成的句子: `"2"` 和 `"4"` 是数字，其他像 `"puppy"` 这样的 tokens 属于单词。

给你一个表示句子的字符串 `s` ，你需要检查 `s` 中的 __全部__ 数字是否从左到右严格递增（即，除了最后一个数字， `s` 中的 __每个__ 数字都严格小于它 __右侧__ 的数字）。

如果满足题目要求，返回 `true` ，否则，返回 `false` 。

__示例:__

示例 1：

![2042-3](https://assets.leetcode.com/uploads/2021/09/30/example1.png)

```text
输入：s = "1 box has 3 blue 4 red 6 green and 12 yellow marbles"
输出：true
解释：句子中的数字是：1, 3, 4, 6, 12 。
这些数字是按从左到右严格递增的 1 < 3 < 4 < 6 < 12 。
```

示例 2：

```text
输入：s = "hello world 5 x 5"
输出：false
解释：句子中的数字是：5, 5 。这些数字不是严格递增的。
```

示例 3：

![2042-4](https://assets.leetcode.com/uploads/2021/09/30/example3.png)

```text
输入：s = "sunset is at 7 51 pm overnight lows will be in the low 50 and 60 s"
输出：false
解释：s 中的数字是：7, 51, 50, 60 。这些数字不是严格递增的。
```

示例 4：

```text
输入：s = "4 5 11 26"
输出：true
解释：s 中的数字是：4, 5, 11, 26 。
这些数字是按从左到右严格递增的：4 < 5 < 11 < 26 。
```

__提示：__

- `3 <= s.length <= 200`
- `s` 由小写英文字母、空格和数字 `0` 到 `9` 组成（包含 `0` 和 `9`）
- `s` 中数字 token 的数目在 `2` 和 `100` 之间（包含 `2` 和 `100`）
- `s` 中的 token 之间由单个空格分隔
- `s` 中至少有 __两个__ 数字
- `s` 中的每个数字都是一个 __小于__ `100` 的 __正__ 数，且不含前导零
- `s` 不含前导或尾随空格

__思路:__

```text
模拟
由于题目中已经规定都为 2-100 之间的正整数，因此可以初始化为 0
遍历字符串，遇到数字则将其转化为整数，与前一个数字比较
若小于等于前一个数字，则返回 false
否则更新前一个数字为当前数字
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool areNumbersAscending(string s) 
    {
        int pre = 0, i = 0, n = s.size();
        while (i < n) 
        {
            if (isdigit(s[i])) 
            {
                int cur = 0;
                while (i < n and isdigit(s[i])) cur = 10 * cur + s[i++] - '0';
                if (cur <= pre) return false;
                pre = cur;
            } 
            else ++i;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean areNumbersAscending(String s) {
        int pre = 0, i = 0, n = s.length();
        while (i < n) {
            if (Character.isDigit(s.charAt(i))) {
                int cur = 0;
                while (i < n && Character.isDigit(s.charAt(i))) cur = 10 * cur + s.charAt(i++) - '0';
                if (cur <= pre) return false;
                pre = cur;
            } else ++i;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def areNumbersAscending(self, s: str) -> bool:
```
