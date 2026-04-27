# 1807 Evaluate the Bracket Pairs of a String 替换字符串中的括号内容

__Description:__

You are given a string `s` that contains some bracket pairs, with each pair containing a __non-empty__ key.

- For example, in the string `"(name)is(age)yearsold"`, there are __two__ bracket pairs that contain the keys `"name"` and `"age"`.

You know the values of a wide range of keys. This is represented by a 2D string array `knowledge` where each `knowledge[i] = [keyi, valuei]` indicates that key `keyi` has a value of `valuei`.

You are tasked to evaluate __all__ of the bracket pairs. When you evaluate a bracket pair that contains some key `keyi`, you will:

- Replace `keyi` and the bracket pair with the key's corresponding `valuei`.
- If you do not know the value of the key, you will replace `keyi` and the bracket pair with a question mark `"?"` (without the quotation marks).

Each key will appear at most once in your `knowledge`. There will not be any nested brackets in `s`.

Return the resulting string after evaluating all of the bracket pairs.

__Example:__

Example 1:

```text
Input: s = "(name)is(age)yearsold", knowledge = [["name","bob"],["age","two"]]
Output: "bobistwoyearsold"
Explanation:
The key "name" has a value of "bob", so replace "(name)" with "bob".
The key "age" has a value of "two", so replace "(age)" with "two".
```

Example 2:

```text
Input: s = "hi(name)", knowledge = [["a","b"]]
Output: "hi?"
Explanation: As you do not know the value of the key "name", replace "(name)" with "?".
```

Example 3:

```text
Input: s = "(a)(a)(a)aaa", knowledge = [["a","yes"]]
Output: "yesyesyesaaa"
Explanation: The same key can appear multiple times.
The key "a" has a value of "yes", so replace all occurrences of "(a)" with "yes".
Notice that the "a"s not in a bracket pair are not evaluated.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `0 <= knowledge.length <= 10 ^ 5`
- `knowledge[i].length == 2`
- `1 <= keyi.length, valuei.length <= 10`
- `s` consists of lowercase English letters and round brackets `'('` and `')'`.
- Every open bracket `'('` in `s` will have a corresponding close bracket `')'`.
- The key in each bracket pair of `s` will be non-empty.
- There will not be any nested bracket pairs in `s`.
- `keyi` and `valuei` consist of lowercase English letters.
- Each `keyi` in `knowledge` is unique.

__题目描述:__

给你一个字符串 `s` ，它包含一些括号对，每个括号中包含一个 __非空__ 的键。

- 比方说，字符串 `"(name)is(age)yearsold"` 中，有 __两个__ 括号对，分别包含键 `"name"` 和 `"age"` 。

你知道许多键对应的值，这些关系由二维字符串数组 `knowledge` 表示，其中 `knowledge[i] = [keyi, valuei]` ，表示键 `keyi` 对应的值为 `valuei` 。

你需要替换 __所有__ 的括号对。当你替换一个括号对，且它包含的键为 `keyi` 时，你需要:

- 将 `keyi` 和括号用对应的值 `valuei` 替换。
- 如果从 `knowledge` 中无法得知某个键对应的值，你需要将 `keyi` 和括号用问号 `"?"` 替换（不需要引号）。

`knowledge` 中每个键最多只会出现一次。 `s` 中不会有嵌套的括号。

请你返回替换 所有 括号对后的结果字符串。

__示例:__

示例 1：

```text
输入：s = "(name)is(age)yearsold", knowledge = [["name","bob"],["age","two"]]
输出："bobistwoyearsold"
解释：
键 "name" 对应的值为 "bob" ，所以将 "(name)" 替换为 "bob" 。
键 "age" 对应的值为 "two" ，所以将 "(age)" 替换为 "two" 。
```

示例 2：

```text
输入：s = "hi(name)", knowledge = [["a","b"]]
输出："hi?"
解释：由于不知道键 "name" 对应的值，所以用 "?" 替换 "(name)" 。
```

示例 3：

```text
输入：s = "(a)(a)(a)aaa", knowledge = [["a","yes"]]
输出："yesyesyesaaa"
解释：相同的键在 s 中可能会出现多次。
键 "a" 对应的值为 "yes" ，所以将所有的 "(a)" 替换为 "yes" 。
注意，不在括号里的 "a" 不需要被替换。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `0 <= knowledge.length <= 10 ^ 5`
- `knowledge[i].length == 2`
- `1 <= keyi.length, valuei.length <= 10`
- `s` 只包含小写英文字母和圆括号 `'('` 和 `')'` 。
- `s` 中每一个左圆括号 `'('` 都有对应的右圆括号 `')'` 。
- `s` 中每对括号内的键都不会为空。
- `s` 中不会有嵌套括号对。
- `keyi` 和 `valuei` 只包含小写英文字母。
- `knowledge` 中的 `keyi` 不会重复。

__思路:__

```text
模拟
用哈希表记录 key 和 value
查询到的括号中的值就进行替换
否则用问号代替
时间复杂度为 O(N + M), 空间复杂度为 O(N + M), 其中 M 为 knowledge 数组的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string evaluate(string s, vector<vector<string>>& knowledge) 
    {
        unordered_map<string, string> m;
        for (const auto& words : knowledge) m[words.front()] = words.back();
        int start = -1, n = s.size();
        string result = "";
        for (int i = 0; i < n; i++)
        {
            if (s[i] == '(') start = i;
            else if (s[i] == ')')
            {
                result += m.count(s.substr(start + 1, i - start - 1)) ? m[s.substr(start + 1, i - start - 1)] : "?";
                start = -1;
            }
            else if (start < 0) result += s[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String evaluate(String s, List<List<String>> knowledge) {
        Map<String, String> map = new HashMap<>();
        for (List<String> list : knowledge) map.put(list.get(0), list.get(1));
        StringBuilder result = new StringBuilder();
        int start = -1, n = s.length();
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '(') start = i;
            else if (s.charAt(i) == ')') {
                result.append(map.getOrDefault(s.substring(start + 1, i), "?"));
                start = -1;
            } else if (start < 0) result.append(s.charAt(i));
        }
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def evaluate(self, s: str, knowledge: List[List[str]]) -> str:
        return s.replace('(', '{').replace(')', '}').format_map(d := defaultdict(lambda: '?', knowledge))
```
