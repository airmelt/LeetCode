# 2232 Minimize Result by Adding Parentheses to Expression 向表达式添加括号后的最小结果

__Description:__

You are given a __0-indexed__ string `expression` of the form `"<num1>+<num2>"` where `<num1>` and `<num2>` represent positive integers.

Add a pair of parentheses to `expression` such that after the addition of parentheses, `expression` is a __valid__ mathematical expression and evaluates to the __smallest__ possible value. The left parenthesis __must__ be added to the left of `'+'` and the right parenthesis __must__ be added to the right of `'+'`.

Return `expression` _after adding a pair of parentheses such that_ `expression` _evaluates to the __smallest__ possible value._ If there are multiple answers that yield the same result, return any of them.

The input has been generated such that the original value of `expression`, and the value of `expression` after adding any pair of parentheses that meets the requirements fits within a signed 32-bit integer.

__Example:__

Example 1:

```text
Input:  expression = "247+38"
Output:  "2(47+38)"
Explanation:  The "expression" evaluates to 2 * (47 + 38) = 2 * 85 = 170.
Note that "2(4)7+38" is invalid because the right parenthesis must be to the right of the '+'.
It can be shown that 170 is the smallest possible value.
```

Example 2:

```text
Input: expression = "12+34"
Output: "1(2+3)4"
Explanation: The "expression" evaluates to 1 * (2 + 3) * 4 = 1 * 5 * 4 = 20.
```

Example 3:

```text
Input:  expression = "999+999"
Output:  "(999+999)"
Explanation:  The "expression" evaluates to 999 + 999 = 1998.
```

__Constraints:__

- `3 <= expression.length <= 10`
- `expression` consists of digits from `'1'` to `'9'` and `'+'`.
- `expression` starts and ends with digits.
- `expression` contains exactly one `'+'`.
- The original value of `expression`, and the value of `expression` after adding any pair of parentheses that meets the requirements fits within a signed 32-bit integer.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `expression` ，格式为 `"<num1>+<num2>"` ，其中 `<num1>` 和 `<num2>` 表示正整数。

请你向 `expression` 中添加一对括号，使得在添加之后， `expression` 仍然是一个有效的数学表达式，并且计算后可以得到 __最小__ 可能值。左括号 __必须__ 添加在 `'+'` 的左侧，而右括号必须添加在 `'+'` 的右侧。

返回添加一对括号后形成的表达式 `expression` ，且满足 `expression` 计算得到 __最小__ 可能值_。_如果存在多个答案都能产生相同结果，返回任意一个答案。

生成的输入满足: `expression` 的原始值和添加满足要求的任一对括号之后 `expression` 的值，都符合 32-bit 带符号整数范围。

__示例:__

示例 1：

```text
输入: expression = "247+38"
输出: "2(47+38)"
解释: 表达式计算得到 2 * (47 + 38) = 2 * 85 = 170 。
注意 "2(4)7+38" 不是有效的结果，因为右括号必须添加在 '+' 的右侧。
可以证明 170 是最小可能值。
```

示例 2：

```text
输入：expression = "12+34"
输出："1(2+3)4"
解释：表达式计算得到 1 * (2 + 3) * 4 = 1 * 5 * 4 = 20 。
```

示例 3：

```text
输入：expression = "999+999"
输出："(999+999)"
解释：表达式计算得到 999 + 999 = 1998 。
```

__提示：__

- `3 <= expression.length <= 10`
- `expression` 仅由数字 `'1'` 到 `'9'` 和 `'+'` 组成
- `expression` 由数字开始和结束
- `expression` 恰好仅含有一个 `'+'`.
- `expression` 的原始值和添加满足要求的任一对括号之后 `expression` 的值，都符合 32-bit 带符号整数范围

__思路:__

```text
模拟
暴力在加号的左边的每个位置添加左括号
在加号的右边的每个位置添加右括号
记录最小值以及括号的位置
时间复杂度为 O(N ^ 3), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string minimizeResult(string expression) 
    {
        int add = -1, n = expression.size(), min_value = INT_MAX, part1 = -1, part2 = -1, cur = 0;
        for (int i = 0; i < n; i++) if (expression[i] == '+') add = i;
        for (int i = 0; i < add; i++) 
        {
            for (int j = add + 2; j <= n; j++) 
            {
                string s1 = expression.substr(0, i), s2 = expression.substr(i, add - i), s3 = expression.substr(add + 1, j - add - 1), s4 = expression.substr(j, n - j);
                if ((cur = (s1.empty() ? 1 : stoi(s1)) * (stoi(s2) + stoi(s3)) * (s4.empty() ? 1 : stoi(s4))) < min_value) 
                {
                    part1 = i;
                    part2 = j;
                    min_value = cur;
                }
            }
        }
        expression.insert(part1, "(");
        expression.insert(part2 + 1, ")");
        return expression;
    }
};
```

__Java__:

```Java
class Solution {
    public String minimizeResult(String expression) {
        int add = expression.indexOf('+'), n = expression.length(), minValue = Integer.MAX_VALUE, part1 = -1, part2 = -1, cur = 0;
        for (int i = 0; i < add; i++) {
            for (int j = add + 2; j <= n; j++) {
                String s1 = expression.substring(0, i), s2 = expression.substring(i, add), s3 = expression.substring(add + 1, j), s4 = expression.substring(j, n);
                if ((cur = ("".equals(s1) ? 1 : Integer.parseInt(s1)) * (Integer.parseInt(s2) + Integer.parseInt(s3)) * ("".equals(s4) ? 1 : Integer.parseInt(s4))) < minValue) {
                    part1 = i;
                    part2 = j;
                    minValue = cur;
                }
            }
        }
        StringBuilder sb = new StringBuilder(expression);
        sb.insert(part1, '(');
        sb.insert(part2 + 1, ')');
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def minimizeResult(self, expression: str) -> str:
        return sorted((eval(expression[:i] or '1') * eval(expression[i:j + 1]) * eval(expression[j + 1:] or '1'), f'{expression[:i]}({expression[i:j + 1]}){expression[j + 1:]}') for i in range(expression.find('+')) for j in range(expression.find('+') + 1, len(expression)))[0][1]
```
