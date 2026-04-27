# 2109 Adding Spaces to a String 向字符串添加空格

__Description:__

You are given a __0-indexed__ string `s` and a __0-indexed__ integer array `spaces` that describes the indices in the original string where spaces will be added. Each space should be inserted __before__ the character at the given index.

- For example, given `s = "EnjoyYourCoffee"` and `spaces = [5, 9]`, we place spaces before `'Y'` and `'C'`, which are at indices `5` and `9` respectively. Thus, we obtain "Enjoy __Y__ our __C__ offee".

Return the modified string after the spaces have been added.

__Example:__

Example 1:

```text
Input: s = "LeetcodeHelpsMeLearn", spaces = [8,13,15]
Output: "Leetcode Helps Me Learn"
Explanation: 
The indices 8, 13, and 15 correspond to the underlined characters in "LeetcodeHelpsMeLearn".
We then place spaces before those characters.
```

Example 2:

```text
Input: s = "icodeinpython", spaces = [1,5,7,9]
Output: "i code in py thon"
Explanation:
The indices 1, 5, 7, and 9 correspond to the underlined characters in "icodeinpython".
We then place spaces before those characters.
```

Example 3:

```text
Input: s = "spacing", spaces = [0,1,2,3,4,5,6]
Output: " s p a c i n g"
Explanation:
We are also able to place spaces before the first character of the string.
```

__Constraints:__

- `1 <= s.length <= 3 * 10 ^ 5`
- `s` consists only of lowercase and uppercase English letters.
- `1 <= spaces.length <= 3 * 10 ^ 5`
- `0 <= spaces[i] <= s.length - 1`
- All the values of `spaces` are __strictly increasing__.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` ，以及一个下标从 __0__ 开始的整数数组 `spaces` 。

数组 `spaces` 描述原字符串中需要添加空格的下标。每个空格都应该插入到给定索引处的字符值 __之前__ 。

- 例如， `s = "EnjoyYourCoffee"` 且 `spaces = [5, 9]` ，那么我们需要在 `'Y'` 和 `'C'` 之前添加空格，这两个字符分别位于下标 `5` 和下标 `9` 。因此，最终得到 "Enjoy ___Y___ our ___C___ offee" 。

请你添加空格，并返回修改后的字符串。

__示例:__

示例 1：

```text
输入：s = "LeetcodeHelpsMeLearn", spaces = [8,13,15]
输出："Leetcode Helps Me Learn"
解释：
下标 8、13 和 15 对应 "LeetcodeHelpsMeLearn" 中加粗斜体字符。
接着在这些字符前添加空格。
```

示例 2：

```text
输入：s = "icodeinpython", spaces = [1,5,7,9]
输出："i code in py thon"
解释：
下标 1、5、7 和 9 对应 "icodeinpython" 中加粗斜体字符。
接着在这些字符前添加空格。
```

示例 3：

```text
输入：s = "spacing", spaces = [0,1,2,3,4,5,6]
输出：" s p a c i n g"
解释：
字符串的第一个字符前可以添加空格。
```

__提示：__

- `1 <= s.length <= 3 * 10 ^ 5`
- `s` 仅由大小写英文字母组成
- `1 <= spaces.length <= 3 * 10 ^ 5`
- `0 <= spaces[i] <= s.length - 1`
- `spaces` 中的所有值 __严格递增__

__思路:__

```text
双指针
用一个指针 i 遍历 s
另一个指针 j 遍历 spaces
当 i 在 j 指向的 spaces 中间的时候, 复制字符
否则移动 j 指针增加空格
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string addSpaces(string s, vector<int>& spaces) 
    {
        string result;
        result.reserve(s.size() + spaces.size());
        for (int i{}, j{}; auto& c : s)
        {
            if (j < spaces.size() and i++ == spaces[j]) 
            {
                result += ' ';
                ++j;
            }
            result += c;
        }
        return result; 
    }
};
```

__Java__:

```Java
class Solution {
    public String addSpaces(String s, int[] spaces) {
        StringBuilder sb = new StringBuilder(s.length() + spaces.length);
        int i = 0;
        for (int space : spaces) {
            sb.append(s, i, space);
            sb.append(' ');
            i = space;
        }
        sb.append(s, i, s.length());
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def addSpaces(self, s: str, spaces: List[int]) -> str:
        c = list(s)
        for i in spaces:
            c[i] = ' ' + c[i]
        return ''.join(c)
```
