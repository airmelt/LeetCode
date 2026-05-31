# 3039 Apply Operations to Make String Empty 进行操作使字符串为空

__Description:__

You are given a string `s`.

Consider performing the following operation until `s` becomes __empty__:

- For __every__ alphabet character from `'a'` to `'z'`, remove the __first__ occurrence of that character in `s` (if it exists).

For example, let initially `s = "aabcbbca"`. We do the following operations:

- Remove the underlined characters s = "__a__ a __bc__ bbca". The resulting string is `s = "abbca"`.
- Remove the underlined characters `s = "__ab__ b __c__ a"`. The resulting string is `s = "ba"`.
- Remove the underlined characters `s = "__ba__"`. The resulting string is `s = ""`.

Return _the value of the string_ `s` _right __before__ applying the __last__ operation_. In the example above, answer is `"ba"`.

__Example:__

Example 1:

```text
Input: s = "aabcbbca"
Output: "ba"
Explanation: Explained in the statement.
```

Example 2:

```text
Input: s = "abcd"
Output: "abcd"
Explanation: We do the following operation:
```

- Remove the underlined characters s = "abcd". The resulting string is s = "".

The string just before the last operation is "abcd".

__Constraints:__

- `1 <= s.length <= 5 * 10 ^ 5`
- `s` consists only of lowercase English letters.

__题目描述:__

给你一个字符串 `s` 。

请你进行以下操作直到 `s` 为 __空__ :

- 每次操作 __依次__ 遍历 `'a'` 到 `'z'`，如果当前字符出现在 `s` 中，那么删除出现位置 __最早__ 的该字符（如果存在的话）。

例如，最初 `s = "aabcbbca"`。我们执行下述操作:

- 移除下划线的字符  s = "__a__ a __bc__ bbca"。结果字符串为 `s = "abbca"`。
- 移除下划线的字符  s = "__ab__ b __c__ a"。结果字符串为 `s = "ba"`。
- 移除下划线的字符  s = "__ba__"。结果字符串为 `s = ""`。

请你返回进行 __最后__ 一次操作 __之前__ 的字符串 `s` 。在上面的例子中，答案是 `"ba"`。

__示例:__

示例 1：

```text
输入：s = "aabcbbca"
输出："ba"
解释：已经在题目描述中解释。
```

示例 2：

```text
输入：s = "abcd"
输出："abcd"
解释：我们进行以下操作：
```

- 删除 s = "abcd" 中加粗加斜字符，得到字符串 s = "" 。

进行最后一次操作之前的字符串为 "abcd" 。

__提示：__

- `1 <= s.length <= 5 * 10 ^ 5`
- `s` 只包含小写英文字母。

__思路:__

```text
贪心
记录每一个字符最后出现的位置
找到字符出现最大的次数 mx
找到出现等于 mx 的所有字符
按照最后出现位置从小到大排序就是结果
时间复杂度为 O(N), 空间复杂度为 O(1), 只需要额外 26 个字母的空间, 排序也只针对 26 个字母排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string lastNonEmptyString(string s) 
    {
        vector<int> last(26), d(26), a;
        for (int i = 0, c = 0, n = s.size(); i < n; i++) 
        {
            last[c = s[i] - 'a'] = i;
            ++d[c];
        }
        for (int i = 0, mx = *max_element(d.begin(), d.end()); i < 26; i++) if (d[i] == mx) a.emplace_back(last[i]);
        sort(a.begin(), a.end());
        string result;
        for (const auto& i : a) result += s[i];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String lastNonEmptyString(String s) {
        int last[] = new int[26], d[] = new int[26], n = s.length();
        for (int i = 0, c = 0; i < n; i++) {
            last[c = s.charAt(i) - 'a'] = i;
            ++d[c];
        }
        var a = new ArrayList<Integer>();
        for (int i = 0, mx = Arrays.stream(d).max().getAsInt(); i < 26; i++) if (d[i] == mx) a.add(last[i]);
        Collections.sort(a);
        return a.stream().map(i -> String.valueOf(s.charAt(i))).collect(Collectors.joining());
    }
}
```

__Python__:

```Python
class Solution:
    def lastNonEmptyString(self, s: str) -> str:
        return ''.join(s[i] for i in a) if (last := {c: i for i, c in enumerate(s)}) and (d := Counter(s)) and (mx := max(d.values())) and (a := sorted(last[k] for k, v in d.items() if v == mx)) else ''
```
