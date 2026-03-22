# 2788 Split Strings by Separator 按分隔符拆分字符串

__Description:__

Given an array of strings `words` and a character `separator`, __split__ each string in `words` by `separator`.

Return an array of strings containing the new strings formed after the splits, excluding empty strings.

Notes

- `separator` is used to determine where the split should occur, but it is not included as part of the resulting strings.
- A split may result in more than two strings.
- The resulting strings must maintain the same order as they were initially given.

__Example:__

Example 1:

```text
Input: words = ["one.two.three","four.five","six"], separator = "."
Output: ["one","two","three","four","five","six"]
Explanation: In this example we split as follows:

"one.two.three" splits into "one", "two", "three"
"four.five" splits into "four", "five"
"six" splits into "six" 

Hence, the resulting array is ["one","two","three","four","five","six"].
```

Example 2:

```text
Input: words = ["$easy$","$problem$"], separator = "$"
Output: ["easy","problem"]
Explanation: In this example we split as follows: 

"$easy$" splits into "easy" (excluding empty strings)
"$problem$" splits into "problem" (excluding empty strings)

Hence, the resulting array is ["easy","problem"].
```

Example 3:

```text
Input: words = ["|||"], separator = "|"
Output: []
Explanation: In this example the resulting split of "|||" will contain only empty strings, so we return an empty array [].
```

__Constraints:__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 20`
- characters in `words[i]` are either lowercase English letters or characters from the string `".,|$#@"` (excluding the quotes)
- `separator` is a character from the string `".,|$#@"` (excluding the quotes)

__题目描述:__

给你一个字符串数组 `words` 和一个字符 `separator` ，请你按 `separator` 拆分 `words` 中的每个字符串。

返回一个由拆分后的新字符串组成的字符串数组，不包括空字符串 。

注意

- `separator` 用于决定拆分发生的位置，但它不包含在结果字符串中。
- 拆分可能形成两个以上的字符串。
- 结果字符串必须保持初始相同的先后顺序。

__示例:__

示例 1：

```text
输入：words = ["one.two.three","four.five","six"], separator = "."
输出：["one","two","three","four","five","six"]
解释：在本示例中，我们进行下述拆分：

"one.two.three" 拆分为 "one", "two", "three"
"four.five" 拆分为 "four", "five"
"six" 拆分为 "six" 

因此，结果数组为 ["one","two","three","four","five","six"] 。
```

示例 2：

```text
输入：words = ["$easy$","$problem$"], separator = "$"
输出：["easy","problem"]
解释：在本示例中，我们进行下述拆分：

"$easy$" 拆分为 "easy"（不包括空字符串）
"$problem$" 拆分为 "problem"（不包括空字符串）

因此，结果数组为 ["easy","problem"] 。
```

示例 3：

```text
输入：words = ["|||"], separator = "|"
输出：[]
解释：在本示例中，"|||" 的拆分结果将只包含一些空字符串，所以我们返回一个空数组 [] 。
```

__提示：__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 20`
- `words[i]` 中的字符要么是小写英文字母，要么就是字符串 `".,|$#@"` 中的字符（不包括引号）
- `separator` 是字符串 `".,|$#@"` 中的某个字符（不包括引号）

__思路:__

```text
模拟
按照分隔符先将字符串拆开
然后只将非空字符串保留到结果数组
时间复杂度为 O(MN), 空间复杂度为 O(M), 其中 N 为 words 数组长度, M 为字符串平均长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> splitWordsBySeparator(vector<string>& words, char separator) 
    {
        vector<string> result;
        for (const auto& word : words) 
        {
            stringstream ss(word);
            string sub;
            while (getline(ss, sub, separator)) if (!sub.empty()) result.push_back(sub);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> splitWordsBySeparator(List<String> words, char separator) {
        return words.stream().flatMap(word -> Arrays.stream(word.split("\\" + String.valueOf(separator)))).filter(word -> !word.isEmpty()).collect(Collectors.toList());
    }
}
```

__Python__:

```Python
class Solution:
    def splitWordsBySeparator(self, words: List[str], separator: str) -> List[str]:
        return [w for w in sum([word.split(separator) for word in words], []) if w]
```
