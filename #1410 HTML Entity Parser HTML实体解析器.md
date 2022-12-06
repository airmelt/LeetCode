# 1410 HTML Entity Parser HTML实体解析器

__Description:__

HTML entity parser is the parser that takes HTML code as input and replace all the entities of the special characters by the characters itself.

The special characters and their entities for HTML are:

- __Quotation Mark:__ the entity is `&quot;` and symbol character is `"`.
- __Single Quote Mark:__ the entity is `&apos;` and symbol character is `'`.
- __Ampersand:__ the entity is `&amp;` and symbol character is `&`.
- __Greater Than Sign:__ the entity is `&gt;` and symbol character is `>`.
- __Less Than Sign:__ the entity is `&lt;` and symbol character is `<`.
- __Slash:__ the entity is `&frasl;` and symbol character is `/`.

Given the input `text` string to the HTML parser, you have to implement the entity parser.

Return the text after replacing the entities by the special characters.

__Example:__

Example 1:

```text
Input: text = "&amp; is an HTML entity but &ambassador; is not."
Output: "& is an HTML entity but &ambassador; is not."
Explanation: The parser will replace the &amp; entity by &
```

Example 2:

```text
Input: text = "and I quote: &quot;...&quot;"
Output: "and I quote: \"...\""
```

__Constraints:__

- `1 <= text.length <= 10 ^ 5`
- The string may contain any possible characters out of all the 256 ASCII characters.

__题目描述:__

「HTML 实体解析器」 是一种特殊的解析器，它将 HTML 代码作为输入，并用字符本身替换掉所有这些特殊的字符实体。

HTML 里这些特殊字符和它们对应的字符实体包括：

- __双引号：__ 字符实体为 `&quot;` ，对应的字符是 `"` 。
- __单引号：__ 字符实体为 `&apos;` ，对应的字符是 `'` 。
- __与符号：__ 字符实体为 `&amp;` ，对应对的字符是 `&` 。
- __大于号：__ 字符实体为 `&gt;` ，对应的字符是 `>` 。
- __小于号：__ 字符实体为 `&lt;` ，对应的字符是 `<` 。
- __斜线号：__ 字符实体为 `&frasl;` ，对应的字符是 `/` 。

给你输入字符串 `text` ，请你实现一个 HTML 实体解析器，返回解析器解析后的结果。

__示例:__

示例 1：

```text
输入：text = "&amp; is an HTML entity but &ambassador; is not."
输出："& is an HTML entity but &ambassador; is not."
解释：解析器把字符实体 &amp; 用 & 替换
```

示例 2：

```text
输入：text = "and I quote: &quot;...&quot;"
输出："and I quote: \"...\""
```

示例 3：

```text
输入：text = "Stay home! Practice on Leetcode :)"
输出："Stay home! Practice on Leetcode :)"
```

示例 4：

```text
输入：text = "x &gt; y &amp;&amp; x &lt; y is always false"
输出："x > y && x < y is always false"
```

示例 5：

```text
输入：text = "leetcode.com&frasl;problemset&frasl;all"
输出："leetcode.com/problemset/all"
```

__提示：__

- `1 <= text.length <= 10 ^ 5`
- 字符串可能包含 256 个ASCII 字符中的任意字符。

__思路:__

```text
模拟
将需要转义的字符串放入哈希表中
遍历 text 字符串
如果扫描到 '&' 和 ';'
记录下出现的位置, 检查子串是否在哈希表中
如果在哈希表中就替换
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string entityParser(string text) 
    {
        unordered_map<string, string> m = {{"&quot;", "\""}, {"&apos;", "'"}, {"&amp;", "&"}, {"&gt;", ">"}, {"&lt;", "<"}, {"&frasl;", "/"}};
        string result = text;
        int start = result.find('&'), end = result.find(';');
        while ((end != -1) and (start != -1)) 
        {
            string cur = result.substr(start, end - start + 1);
            if (m.count(cur)) result.replace(result.begin() + start, result.begin() + end + 1, m[cur]);
            start = result.find('&', start + 1);
            end = result.find(';', start + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String entityParser(String text) {
        return text.replaceAll("&quot;", "\"").replaceAll("&apos;", "'").replaceAll("&gt;", ">").replaceAll("&lt;", "<").replaceAll("&frasl;", "/").replaceAll("&amp;", "&");
    }
}
```

__Python__:

```Python
class Solution:
    def entityParser(self, text: str) -> str:
        return text.replace('&quot;', '\"').replace('&apos;', '\'').replace('&gt;', '>').replace('&lt;', '<').replace('&frasl;', '/').replace('&amp;', '&')
```
