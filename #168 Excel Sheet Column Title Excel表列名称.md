__Description__:
Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:

    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB
    ...
__Example__:
Example 1:
Input: 1
Output: "A"

Example 2:
Input: 28
Output: "AB"

Example 3:
Input: 701
Output: "ZY"

__题目描述__:
给定一个正整数，返回它在 Excel 表中相对应的列名称。

例如，

    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB
    ...
__示例__:
示例 1:
输入: 1
输出: "A"

示例 2:
输入: 28
输出: "AB"

示例 3:
输入: 701
输出: "ZY"

__思路__:
不断取 26的余数和商即可, 注意边界条件
时间复杂度O(logn), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    string convertToTitle(int n) {
        string result = "";
        while (n) {
            n--;
            result += n % 26 + 'A';
            n /= 26;
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

__Java__:
```
class Solution {
    public String convertToTitle(int n) {
        return n == 0 ? "" : convertToTitle(--n / 26) + (char)(n % 26 + 'A');
    }
}
```

__Python__:
```
class Solution:
    def convertToTitle(self, n: int) -> str:
        if not n:
            return ""
        n -= 1
        return self.convertToTitle(n // 26) + chr(n % 26 + ord('A'))
```
