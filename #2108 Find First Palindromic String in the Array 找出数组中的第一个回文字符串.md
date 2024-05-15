# 2108 Find First Palindromic String in the Array 找出数组中的第一个回文字符串

__Description:__

Given an array of strings `words`, return _the first __palindromic__ string in the array_. If there is no such string, return _an __empty string___ `""`.

A string is palindromic if it reads the same forward and backward.

__Example:__

Example 1:

```text
Input: words = ["abc","car","ada","racecar","cool"]
Output: "ada"
Explanation: The first string that is palindromic is "ada".
Note that "racecar" is also palindromic, but it is not the first.
```

Example 2:

```text
Input: words = ["notapalindrome","racecar"]
Output: "racecar"
Explanation: The first and only string that is palindromic is "racecar".
```

Example 3:

```text
Input: words = ["def","ghi"]
Output: ""
Explanation: There are no palindromic strings, so the empty string is returned.
```

__Constraints:__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` consists only of lowercase English letters.

__题目描述:__

给你一个字符串数组 `words` ，找出并返回数组中的 __第一个回文字符串__ 。如果不存在满足要求的字符串，返回一个 __空字符串__ `""` 。

回文字符串 的定义为：如果一个字符串正着读和反着读一样，那么该字符串就是一个 回文字符串 。

__示例:__

示例 1：

```text
输入：words = ["abc","car","ada","racecar","cool"]
输出："ada"
解释：第一个回文字符串是 "ada" 。
注意，"racecar" 也是回文字符串，但它不是第一个。
```

示例 2：

```text
输入：words = ["notapalindrome","racecar"]
输出："racecar"
解释：第一个也是唯一一个回文字符串是 "racecar" 。
```

示例 3：

```text
输入：words = ["def","ghi"]
输出：""
解释：不存在回文字符串，所以返回一个空字符串。
```

__提示：__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` 仅由小写英文字母组成

__思路:__

```text
模拟
遍历数组 words, 对于每个字符串 word, 判断其是否为回文字符串
若是, 则返回该字符串
若否, 则返回空字符串
判断回文字符串的方法为, 从两端向中间遍历, 若两端字符不相等, 则返回 false, 否则返回 true
时间复杂度为 O(MN), 空间复杂度为 O(1), 其中 M 为字符串的平均长度, N 为字符串的个数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string firstPalindrome(vector<string>& words) 
    {
        auto helper = [](const auto& word) -> bool 
        {
            int n = word.size(), left = 0, right = n - 1;
            while (left < right) if (word[left++] != word[right--]) return false;
            return true;
        };
        for (const auto& word: words) if (helper(word)) return word;
        return "";
    }
};
```

__Java__:

```Java
class Solution {
    public String firstPalindrome(String[] words) {
        for (String word: words) if (helper(word)) return word;
        return "";
    }

    private boolean helper(String word) {
        int n = word.length(), left = 0, right = n - 1;
        while (left < right) if (word.charAt(left++) != word.charAt(right--)) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def firstPalindrome(self, words: List[str]) -> str:
        return next((s for s in words if s == s[::-1]), "")
```
