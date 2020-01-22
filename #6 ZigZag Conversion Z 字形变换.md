__Description__:
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);
__Example:__
Example 1:

Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"

Example 2:

Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
```
P     I    N
A   L S  I G
Y A   H R
P     I
```

__题目描述__:
将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
```
L   C   I   R
E T O E S I I G
E   D   H   N
```
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);
__示例 :__
示例 1:

输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"

示例 2:

输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:
```
L     D     R
E   O E   I I
E C   I H   N
T     S     G
```

__思路__:
1. 按 Z字形遍历字符串, 记录当前遍历的行数, 如果遍历到给定的行数(0/numRows)就反向
2. 写出 Z字形字符串的下标, 找到下标的规律, 分成第一行/最后一行和其他行, 按照下标输出
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    string convert(string s, int numRows) 
    {
        if (numRows == 1) return s;
        string result;
        int step = 2 * numRows - 2;
        for (int i = 0; i < numRows; i++) 
        {
            for (int j = 0; j + i < s.size(); j += step) 
            {
                result += s[j + i];
                if (i && i != numRows - 1 && j + step - i < s.size()) result += s[j + step - i];
            }
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1) return s;
        List<StringBuilder> rows = new ArrayList<>();
        for (int i = 0; i < Math.min(numRows, s.length()); i++) rows.add(new StringBuilder());
        int cur = 0;
        boolean next = false;
        for (char c : s.toCharArray()) {
            rows.get(cur).append(c);
            if (cur == 0 || cur == numRows - 1) next = !next;
            cur += next ? 1 : -1;
        }
        StringBuilder result = new StringBuilder();
        for (StringBuilder row : rows) result.append(row);
        return result.toString();
    }
}
```

__Python__:
```Python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1:
            return s;
        result, step = '', 2 * numRows - 2
        for i in range(numRows):
            j = 0
            while i + j < len(s):
                result += s[i + j]
                if i and i != numRows - 1 and j + step - i < len(s):
                    result += s[j + step - i]
                j += step
        return result
```