# 816 Ambiguous Coordinates 模糊坐标

__Description__:
We had some 2-dimensional coordinates, like "(1, 3)" or "(2, 0.5)". Then, we removed all commas, decimal points, and spaces and ended up with the string s.

For example, "(1, 3)" becomes s = "(13)" and "(2, 0.5)" becomes s = "(205)".
Return a list of strings representing all possibilities for what our original coordinates could have been.

Our original representation never had extraneous zeroes, so we never started with numbers like "00", "0.0", "0.00", "1.0", "001", "00.01", or any other number that can be represented with fewer digits. Also, a decimal point within a number never occurs without at least one digit occurring before it, so we never started with numbers like ".1".

The final answer list can be returned in any order. All coordinates in the final answer have exactly one space between them (occurring after the comma.)

__Example:__

Example 1:

Input: s = "(123)"
Output: ["(1, 2.3)","(1, 23)","(1.2, 3)","(12, 3)"]

Example 2:

Input: s = "(0123)"
Output: ["(0, 1.23)","(0, 12.3)","(0, 123)","(0.1, 2.3)","(0.1, 23)","(0.12, 3)"]
Explanation: 0.0, 00, 0001 or 00.01 are not allowed.

Example 3:

Input: s = "(00011)"
Output: ["(0, 0.011)","(0.001, 1)"]

Example 4:

Input: s = "(100)"
Output: ["(10, 0)"]
Explanation: 1.0 is not allowed.

__Constraints:__

4 <= s.length <= 12
s[0] == '(' and s[s.length - 1] == ')'.
The rest of s are digits.

__题目描述__:
我们有一些二维坐标，如 "(1, 3)" 或 "(2, 0.5)"，然后我们移除所有逗号，小数点和空格，得到一个字符串S。返回所有可能的原始字符串到一个列表中。

原始的坐标表示法不会存在多余的零，所以不会出现类似于"00", "0.0", "0.00", "1.0", "001", "00.01"或一些其他更小的数来表示坐标。此外，一个小数点前至少存在一个数，所以也不会出现“.1”形式的数字。

最后返回的列表可以是任意顺序的。而且注意返回的两个数字中间（逗号之后）都有一个空格。

__示例 :__

示例 1:
输入: "(123)"
输出: ["(1, 23)", "(12, 3)", "(1.2, 3)", "(1, 2.3)"]

示例 2:
输入: "(00011)"
输出:  ["(0.001, 1)", "(0, 0.011)"]
解释:
0.0, 00, 0001 或 00.01 是不被允许的。

示例 3:
输入: "(0123)"
输出: ["(0, 123)", "(0, 12.3)", "(0, 1.23)", "(0.1, 23)", "(0.1, 2.3)", "(0.12, 3)"]

示例 4:
输入: "(100)"
输出: [(10, 0)]
解释:
1.0 是不被允许的。

__提示:__

4 <= S.length <= 12.
S[0] = "(", S[S.length - 1] = ")", 且字符串 S 中的其他元素都是数字。

__思路__:

模拟
将数字分别分成 left 和 right 两份
尝试给两个数字加上小数点
left 和 right 再生成小数
将 left 前导 0 和 right 结尾 0 剪枝
时间复杂度为 O(n ^ 3), 空间复杂度为 O(n ^ 3)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> ambiguousCoordinates(string s) 
    {
        vector<string> result;
        for (int i = 2, n = s.size(); i < n - 1; i++) for (const auto& left : helper(s, 1, i)) for (const auto& right : helper(s, i, n - 1)) result.emplace_back("(" + left + ", " + right + ")");
        return result;
    }
private:
    vector<string> helper(string &s, int i, int j) 
    {
        vector<string> result;
        for (int k = 1; k <= j - i; k++)
        {
            string left = s.substr(i, k), right = s.substr(i + k, j - i - k);
            if ((left.front() != '0' or left == "0") and right.back() != '0') result.emplace_back(left + (k < j - i ? "." : "") + right);
            
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> ambiguousCoordinates(String s) {
        List<String> result = new ArrayList<>();
        for (int i = 2, n = s.length(); i < n - 1; i++) for (String left : helper(s, 1, i)) for (String right : helper(s, i, n - 1)) result.add("(" + left + ", " + right + ")");
        return result;
    }
    
    private List<String> helper(String s, int i, int j) {
        List<String> result = new ArrayList<>();
        for (int k = 1; k <= j - i; k++) {
            String left = s.substring(i, i + k), right = s.substring(i + k, j);
            if ((!left.startsWith("0") || "0".equals(left)) && !right.endsWith("0")) result.add(left + (k < j - i ? "." : "") + right);
        }
        return result;
    } 
}
```

__Python__:

```Python
class Solution:
    def ambiguousCoordinates(self, s: str) -> List[str]:
        def helper(cur: str) -> str:
            n = len(cur)
            for i in range(1, n + 1):
                left, right = cur[:i], cur[i:]
                if ((not left.startswith('0') or left == '0') and (not right.endswith('0'))):
                    yield left + ('.' if i != n else '') + right

        s, n = s[1:-1], len(s) - 2
        return ["({}, {})".format(*c) for i in range(1, n) for c in itertools.product(helper(s[:i]), helper(s[i:]))]
```
