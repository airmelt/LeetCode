# 640 Solve the Equation 求解方程

__Description__:
Solve a given equation and return the value of 'x' in the form of a string "x=#value". The equation contains only '+', '-' operation, the variable 'x' and its coefficient. You should return "No solution" if there is no solution for the equation, or "Infinite solutions" if there are infinite solutions for the equation.

If there is exactly one solution for the equation, we ensure that the value of 'x' is an integer.

__Example:__

Example 1:

Input: equation = "x+5-3+x=6+x-2"
Output: "x=2"

Example 2:

Input: equation = "x=x"
Output: "Infinite solutions"

Example 3:

Input: equation = "2x=x"
Output: "x=0"

Example 4:

Input: equation = "2x+3x-6x=x+2"
Output: "x=-1"

Example 5:

Input: equation = "x=x+2"
Output: "No solution"

__Constraints:__

3 <= equation.length <= 1000
equation has exactly one '='.
equation consists of integers with an absolute value in the range [0, 100] without any leading zeros, and the variable 'x'.

__题目描述__:
求解一个给定的方程，将x以字符串"x=#value"的形式返回。该方程仅包含'+'，' - '操作，变量 x 和其对应系数。

如果方程没有解，请返回“No solution”。

如果方程有无限解，则返回“Infinite solutions”。

如果方程中只有一个解，要保证返回值 x 是一个整数。

__示例 :__

示例 1：

输入: "x+5-3+x=6+x-2"
输出: "x=2"

示例 2:

输入: "x=x"
输出: "Infinite solutions"

示例 3:

输入: "2x=x"
输出: "x=0"

示例 4:

输入: "2x+3x-6x=x+2"
输出: "x=-1"

示例 5:

输入: "x=x+2"
输出: "No solution"

__思路__:

模拟解方程
将 x 的系数和常数项分别求和, 可以将 '-' 替换为 '+-', '+x', '-x' 替换为 '+1x', '-1x' 方便后续处理
按照 '=' 分别求 ax = b 中的 a 和 b, 注意正负号即可
最后按 a != 0, b == 0 和其他讨论方程的解
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    pair<int, int> calculate(string str)
    {
        int a = 0, b = 0, sign = 1;
        if (str[0] != '+' and str[0] != '-') str = '+' + str;
        int n = str.size();
        for (int i = 0; i < n; i++)
        {
            if (str[i] == '+') sign = 1;
            else if (str[i] == '-') sign = -1;
            else if (str[i] == 'x')
            {
                if (sign == 1) ++a;
                else --a;
            }
            else
            {
                int j = i;
                int t = 0;
                while (j < n and isdigit(str[j])) t = t * 10 + (str[j++] - '0');
                t = sign * t;
                if (str[j] == 'x')
                {
                    a += t;
                    i = j;
                }
                else
                {
                    b += t;
                    i = j - 1;
                }
            }
        }
        return {a, b};
    }
public:
    string solveEquation(string equation) 
    {
        int equal = equation.find('=');
        string left = equation.substr(0, equal), right = equation.substr(equal + 1);
        auto left_pair = calculate(left), right_pair = calculate(right);
        int a = left_pair.first - right_pair.first, b =  right_pair.second - left_pair.second;
        if (a) return "x=" + to_string(b / a);
        else if (!b) return "Infinite solutions";
        return "No solution";
    }
};
```

__Java__:

```Java
class Solution {
    public String solveEquation(String equation) {
        int a = 0, b = 0, sign = 1;
        for (String f: equation.split("=")) {
            f = f.replace("-", "+-");
            for (String num: f.split("\\+")) {
                if (num.isEmpty()) continue;
                if (num.contains("x")) {
                    if (num.length() == 1) a += sign;
                    else if (num.length() == 2 && num.charAt(0) == '-') a += -sign;
                    else a += sign * Integer.parseInt(num.substring(0, num.length() - 1));
                }
                else b -= sign * Integer.parseInt(num);
            }
            sign = -1;
        }
        if (a != 0) return "x=" + b / a;
        else if (b == 0) return "Infinite solutions";
        return "No solution";
    }
}
```

__Python__:

```Python
class Solution:
    def solveEquation(self, equation: str) -> str:
        e = equation.replace('-x', '-1x').replace('-', '+-')
        a = b = 0
        for i, f in enumerate(e.split('=')):
            if f[0] != '+':
                f = '+' + f
            f = f.replace('+x', '+1x')
            for t in f.split('+'):
                if not t:
                    continue
                if 'x' in t:
                    a += (1 - 2 * i) * int(t[:len(t) - 1])
                else:
                    b -= (1 - 2 * i) * int(t)
        if a:
            return 'x=' + str(b // a)
        elif not b:
            return 'Infinite solutions'
        return 'No solution'
```
