__Description__:
Validate if a given string can be interpreted as a decimal number.

__Example:__
Some examples:
"0" => true
" 0.1 " => true
"abc" => false
"1 a" => false
"2e10" => true
" -90e3   " => true
" 1e" => false
"e3" => false
" 6e-1" => true
" 99e2.5 " => false
"53.5e93" => true
" --6 " => false
"-+3" => false
"95a54e53" => false

__Note:__ It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one. However, here is a list of characters that can be in a valid decimal number:

Numbers 0-9
Exponent - "e"
Positive/negative sign - "+"/"-"
Decimal point - "."
Of course, the context of these characters also matters in the input.

__Update (2015-02-10):__
The signature of the C++ function had been updated. If you still see your function signature accepts a const char * argument, please click the reload button to reset your code definition.

__题目描述__:
验证给定的字符串是否可以解释为十进制数字。

__示例 :__
例如:

"0" => true
" 0.1 " => true
"abc" => false
"1 a" => false
"2e10" => true
" -90e3   " => true
" 1e" => false
"e3" => false
" 6e-1" => true
" 99e2.5 " => false
"53.5e93" => true
" --6 " => false
"-+3" => false
"95a54e53" => false

__说明:__ 我们有意将问题陈述地比较模糊。在实现代码之前，你应当事先思考所有可能的情况。这里给出一份可能存在于有效十进制数字中的字符列表：

数字 0-9
指数 - "e"
正/负号 - "+"/"-"
小数点 - "."
当然，在输入中，这些字符的上下文也很重要。

__更新于 2015-02-10:__
C++函数的形式已经更新了。如果你仍然看见你的函数接收 const char * 类型的参数，请点击重载按钮重置你的代码。

__思路__:
从左到右依次扫描
1. 去掉 s左边的空格
2. 判断在小数点或者指数(e)之前的数为带符号整数
3. 如果是小数点, 要求后面的没有数字(用或“||”表示)或者是无符号整数
4. 如果是指数, 要求后面的数字有且是无符号整数
5. 去掉 s右边的空格
返回是否扫描到了 s的结尾和是否满足数字的 bool值
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    bool isNumber(string s) 
    {
        if (s == "") return false;
        int i = 0, n = s.size();
        scan_space(s, i, n);
        bool is_num = scan_integer(s, i, n);
        if (s[i] == '.')
        {
            ++i;
            is_num = scan_unsigned(s, i, n) || is_num;
        }
        if (s[i] == 'e' || s[i] == 'E')
        {
            ++i;
            is_num = scan_integer(s, i, n) && is_num;
        }
        scan_space(s, i, n);
        return is_num && i == n;
    }
private:
    void scan_space(string &s, int &i, int &n)
    {
        while (i < n && s[i] == ' ') ++i;
    }
    bool scan_integer(string &s, int &i, int &n)
    {
        if (s[i] == '+' || s[i] == '-') ++i;
        return scan_unsigned(s, i, n);
    }
    bool scan_unsigned(string &s, int &i, int &n)
    {
        bool result = false;
        while (i < n && s[i] >= '0' && s[i] <= '9')
        {
            ++i;
            result = true;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public boolean isNumber(String s) {
        return (s == null || s.length() == 0) ? false : (s.matches("(^\\s*[+-]?\\d+(\\.\\d*)?(e[+-]?\\d+)?\\s*$)|(^\\s*[+-]?\\.\\d+(e[+-]?+\\d+)?\\s*$)") ? true : false);
    }
}
```

__Python__:
```Python
class Solution:
    def isNumber(self, s: str) -> bool:
        try:
            num = float(s)
            return True
        except:
            return False
```