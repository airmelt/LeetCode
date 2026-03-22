# 984 String Without AAA or BBB 不含 AAA 或 BBB 的字符串

__Description__:
Given two integers a and b, return any string s such that:

s has length a + b and contains exactly a 'a' letters, and exactly b 'b' letters,
The substring 'aaa' does not occur in s, and
The substring 'bbb' does not occur in s.

__Example:__

Example 1:

Input: a = 1, b = 2
Output: "abb"
Explanation: "abb", "bab" and "bba" are all correct answers.

Example 2:

Input: a = 4, b = 1
Output: "aabaa"

__Constraints:__

0 <= a, b <= 100
It is guaranteed such an s exists for the given a and b.

__题目描述__:
给定两个整数 A 和 B，返回任意字符串 S，要求满足：

S 的长度为 A + B，且正好包含 A 个 'a' 字母与 B 个 'b' 字母；
子串 'aaa' 没有出现在 S 中；
子串 'bbb' 没有出现在 S 中。

__示例 :__

示例 1：

输入：A = 1, B = 2
输出："abb"
解释："abb", "bab" 和 "bba" 都是正确答案。

示例 2：

输入：A = 4, B = 1
输出："aabaa"

__提示:__

0 <= A <= 100
0 <= B <= 100
对于给定的 A 和 B，保证存在满足要求的 S。

__思路__:

贪心
如果 b == 0 输出 a 个 'a'
如果 a == 0 输出 b 个 'b'
如果 a == b 输出一个 "ab"
否则优先拼一个多的 "xxy"
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string strWithout3a3b(int a, int b) 
    {
        return !b ? string(a, 'a') : (!a ? string(b, 'b') : (a == b ? "ab" + strWithout3a3b(a - 1, b - 1) : (a > b ? "aab" + strWithout3a3b(a - 2, b - 1) : "bba" + strWithout3a3b(a - 1, b - 2))));
    }
};
```

__Java__:

```Java
class Solution {
    public String strWithout3a3b(int a, int b) {
        int maxValue = a >= b ? a : b, minValue = a < b ? a : b;
        StringBuilder sb = new StringBuilder();
        String d = a >= b ? "aa" : "bb", s = a >= b ? "b" : "a";
        while (maxValue > 0) {
            sb.append(d).append(s);
            maxValue -= 2;
            --minValue;
        }
        int i = 2;
        while (minValue > 0) {
            sb.insert(i, s);
            i += 4;
            --minValue;
        }
        if (maxValue == -1) sb.delete(0, 1);
        if (minValue == -1) sb.deleteCharAt(sb.length() - 1);
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def strWithout3a3b(self, a: int, b: int) -> str:
        return 'a' * a if not b else 'b' * b if not a else 'ab' + self.strWithout3a3b(a - 1, b - 1) if a == b else 'aab' + self.strWithout3a3b(a - 2, b - 1) if a > b else 'bba' + self.strWithout3a3b(a - 1, b - 2)
```
