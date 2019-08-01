__Description__:
Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.

__Note:__

All letters in hexadecimal (a-f) must be in lowercase.
The hexadecimal string must not contain extra leading 0s. If the number is zero, it is represented by a single zero character '0'; otherwise, the first character in the hexadecimal string will not be the zero character.
The given number is guaranteed to fit within the range of a 32-bit signed integer.
You must not use any method provided by the library which converts/formats the number to hex directly.

__Example:__
Example 1:

Input:
26

Output:
"1a"

Example 2:

Input:
-1

Output:
"ffffffff"

__题目描述__:
给定一个整数，编写一个算法将这个数转换为十六进制数。对于负整数，我们通常使用 补码运算 方法。

__注意:__

十六进制中所有字母(a-f)都必须是小写。
十六进制字符串中不能包含多余的前导零。如果要转化的数为0，那么以单个字符'0'来表示；对于其他情况，十六进制字符串中的第一个字符将不会是0字符。 
给定的数确保在32位有符号整数范围内。
不能使用任何由库提供的将数字直接转换或格式化为十六进制的方法。

__示例：__
示例 1：

输入:
26

输出:
"1a"

示例 2：

输入:
-1

输出:
"ffffffff"

__思路__:
每 4位转换成一位 16进制的数
注意负数在计算机中存储是补码要转换成原码
时间复杂度O(logn), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    string toHex(int num) {
        if (!num) return "0";
		string hex = "0123456789abcdef", result = "";
		while (num && result.size() < 8) {
			result = hex[num & 0xf] + result;
			num >>= 4;
		}
		return result;
    }
};
```

__Java__:
```
class Solution {
    public String toHex(int num) {
        if (num == 0) return "0";
		String hex = "0123456789abcdef", result = "";
		while (num != 0 && result.length() < 8) {
			result = hex.charAt(num & 0xf) + result;
			num >>= 4;
		}
		return result;
    }
}
```

__Python__:
```
class Solution:
    def toHex(self, num: int) -> str:
        return hex(num)[2:] if num >= 0 else hex((1 << 32) + num)[2:]
```
