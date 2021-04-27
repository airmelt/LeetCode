# 592 Fraction Addition and Subtraction 分数加减运算

__Description__:
Given a string representing an expression of fraction addition and subtraction, you need to return the calculation result in string format. The final result should be [irreducible fraction](https://en.wikipedia.org/wiki/Irreducible_fraction). If your final result is an integer, say `2`, you need to change it to the format of fraction that has denominator `1`. So in this case, `2` should be converted to `2/1`.

__Example:__

**Example 1:**
**Input:**"-1/2+1/2"
**Output:** "0/1"

**Example 2:**
**Input:**"-1/2+1/2+1/3"
**Output:** "1/3"

**Example 3:**
**Input:**"1/3-1/2"
**Output:** "-1/6"

**Example 4:**
**Input:**"5/3+1/3"
**Output:** "2/1"

**Note:**

1. The input string only contains `'0'` to `'9'`, `'/'`, `'+'` and `'-'`. So does the output.
2. Each fraction (input and output) has format `±numerator/denominator`. If the first input fraction or the output is positive, then `'+'` will be omitted.
3. The input only contains valid **irreducible fractions**, where the **numerator** and **denominator** of each fraction will always be in the range [1,10]. If the denominator is 1, it means this fraction is actually an integer in a fraction format deined above.
4. The number of given fractions will be in the range [1,10].
5. The numerator and denominator of the **final result** are guaranteed to be valid and in the range of 32-bit int.

__题目描述__:
给定一个表示分数加减运算表达式的字符串，你需要返回一个字符串形式的计算结果。 这个结果应该是不可约分的分数，即最简分数。 如果最终结果是一个整数，例如 2，你需要将它转换成分数形式，其分母为 1。所以在上述例子中, 2 应该被转换为 2/1。

__示例 :__

示例 1:

输入:"-1/2+1/2"
输出: "0/1"

示例 2:

输入:"-1/2+1/2+1/3"
输出: "1/3"

示例 3:

输入:"1/3-1/2"
输出: "-1/6"

示例 4:

输入:"5/3+1/3"
输出: "2/1"

__说明:__

输入和输出字符串只包含 '0' 到 '9' 的数字，以及 '/', '+' 和 '-'。
输入和输出分数格式均为 ±分子/分母。如果输入的第一个分数或者输出的分数是正数，则 '+' 会被省略掉。
输入只包含合法的最简分数，每个分数的分子与分母的范围是  [1,10]。 如果分母是1，意味着这个分数实际上是一个整数。
输入的分数个数范围是 [1,10]。
最终结果的分子与分母保证是 32 位整数范围内的有效整数。

__思路__:

将所有的负号转化为 "+-", 处理字符串的开头
分别将分子和分母按 '/' 分开到两个数组中, 同时记录下分母的乘积
计算分子的和
取分子的和及分母乘积的最大公约数并化简, 可以取绝对值简化计算
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
inline int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
public:
    string fractionAddition(string expression) 
    {
        find_and_replace_all(expression, "-", "+-");
        if (expression[0] != '+') expression = '+' + expression;
        vector<int> numerators, denumerators;
        int n = expression.size(), s = 1, result = 0, i = 0, j = 0;
        while (i < n)
        {
            j = i + 1;
            while (expression[j] != '/') ++j;
            numerators.emplace_back(stoi(expression.substr(i + 1, j - i)));
            i = j;
            j = i + 1;
            while (j < n and expression[j] != '+') ++j;
            denumerators.emplace_back(stoi(expression.substr(i + 1, j - i)));
            i = j;
            s *= denumerators.back();
        }
        for (i = 0, n = numerators.size(); i < n; i++) result += s / denumerators[i] * numerators[i];
        int g = gcd(abs(result), abs(s));
        return to_string(result / g) + "/" + to_string(s / g);
    }
private:
    void find_and_replace_all(string &s, const string &match, const string &new_string)
    {
        size_t pos = s.find(match);
        while (pos != string::npos) 
        {
            s.replace(pos, match.size(), new_string);
            pos = s.find(match, pos + new_string.size());
        }
    }
};
```

__Java__:

```Java
class Solution {
    public String fractionAddition(String expression) {
        expression = expression.replace("-", "+-");
        if (expression.charAt(0) == '+') expression = expression.substring(1);
        String[] nums = expression.split("\\+");
        int numerators[] = new int[nums.length], denumerators[] = new int[nums.length], n = nums.length, s = 1, result = 0;
        for (int i = 0; i < n; i++) {
            String num[] = nums[i].split("/");
            numerators[i] = Integer.parseInt(num[0]);
            denumerators[i] = Integer.parseInt(num[1]);
            s *= denumerators[i];
        }
        for (int i = 0; i < n; i++) result += s / denumerators[i] * numerators[i];
        int g = gcd(Math.abs(result), Math.abs(s));
        return result / g + "/" + s / g;
    }
    
    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
import fractions
class Solution:
    def fractionAddition(self, expression: str) -> str:
        return '{0.numerator}/{0.denominator}'.format(sum(map(fractions.Fraction, re.findall(r'[+\-]?\d+/\d+', expression))))
```
