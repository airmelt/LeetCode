# 2129 Capitalize the Title 将标题首字母大写

__Description:__

You are given a string `title` consisting of one or more words separated by a single space, where each word consists of English letters. __Capitalize__ the string by changing the capitalization of each word such that:

- If the length of the word is `1` or `2` letters, change all letters to lowercase.
- Otherwise, change the first letter to uppercase and the remaining letters to lowercase.

Return _the __capitalized___ `title`.

__Example:__

Example 1:

```text
Input: title = "capiTalIze tHe titLe"
Output: "Capitalize The Title"
Explanation:
Since all the words have a length of at least 3, the first letter of each word is uppercase, and the remaining letters are lowercase.
```

Example 2:

```text
Input: title = "First leTTeR of EACH Word"
Output: "First Letter of Each Word"
Explanation:
The word "of" has length 2, so it is all lowercase.
The remaining words have a length of at least 3, so the first letter of each remaining word is uppercase, and the remaining letters are lowercase.
```

Example 3:

```text
Input: title = "i lOve leetcode"
Output: "i Love Leetcode"
Explanation:
The word "i" has length 1, so it is lowercase.
The remaining words have a length of at least 3, so the first letter of each remaining word is uppercase, and the remaining letters are lowercase.
```

__Constraints:__

- `1 <= title.length <= 100`
- `title` consists of words separated by a single space without any leading or trailing spaces.
- Each word consists of uppercase and lowercase English letters and is __non-empty__.

__题目描述:__

给你一个字符串 `title` ，它由单个空格连接一个或多个单词组成，每个单词都只包含英文字母。请你按以下规则将每个单词的首字母 __大写__ :

- 如果单词的长度为 `1` 或者 `2` ，所有字母变成小写。
- 否则，将单词首字母大写，剩余字母变成小写。

请你返回 __大写后__ 的 `title` 。

__示例:__

示例 1：

```text
输入：title = "capiTalIze tHe titLe"
输出："Capitalize The Title"
解释：
由于所有单词的长度都至少为 3 ，将每个单词首字母大写，剩余字母变为小写。
```

示例 2：

```text
输入：title = "First leTTeR of EACH Word"
输出："First Letter of Each Word"
解释：
单词 "of" 长度为 2 ，所以它保持完全小写。
其他单词长度都至少为 3 ，所以其他单词首字母大写，剩余字母小写。
```

示例 3：

```text
输入：title = "i lOve leetcode"
输出："i Love Leetcode"
解释：
单词 "i" 长度为 1 ，所以它保留小写。
其他单词长度都至少为 3 ，所以其他单词首字母大写，剩余字母小写。
```

__提示：__

- `1 <= title.length <= 100`
- `title` 由单个空格隔开的单词组成，且不含有任何前导或后缀空格。
- 每个单词由大写和小写英文字母组成，且都是 __非空__ 的。

__思路:__

```text
模拟
找到每个空格的位置
分割字符串
按照题意修改字符串的大小写
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string capitalizeTitle(string title) 
    {
        int n = title.size(), l = 0, r = 0;
        while (r < n) 
        {
            while (r < n and title[r] != ' ') ++r;
            if (r - l > 2) title[l++] = toupper(title[l]);
            while (l < r) title[l++] = tolower(title[l]);
            l = ++r;
        }
        return title;
    }
};
```

__Java__:

```Java
class Solution {
    public String capitalizeTitle(String title) {
        StringBuilder sb = new StringBuilder(title);
        int n = title.length(), l = 0, r = 0;
        while (r < n) {
            while (r < n && sb.charAt(r) != ' ') ++r;
            if (r - l > 2) sb.setCharAt(l, Character.toUpperCase(sb.charAt(l++)));
            while (l < r) sb.setCharAt(l, Character.toLowerCase(sb.charAt(l++)));
            l = r++ + 1;
        }
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def capitalizeTitle(self, title: str) -> str:
        return ' '.join(s.title() if len(s) > 2 else s.lower() for s in title.split())
```
