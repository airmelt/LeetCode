# 2384 Largest Palindromic Number 最大回文数字

__Description:__

You are given a string `num` consisting of digits only.

Return _the __largest palindromic__ integer (in the form of a string) that can be formed using digits taken from_ `num`. It should not contain __leading zeroes__.

Notes:

- You do __not__ need to use all the digits of `num`, but you must use __at least__ one digit.
- The digits can be reordered.

__Example:__

Example 1:

```text
Input: num = "444947137"
Output: "7449447"
Explanation: 
Use the digits "4449477" from "444947137" to form the palindromic integer "7449447".
It can be shown that "7449447" is the largest palindromic integer that can be formed.
```

Example 2:

```text
Input: num = "00009"
Output: "9"
Explanation: 
It can be shown that "9" is the largest palindromic integer that can be formed.
Note that the integer returned should not contain leading zeroes.
```

__Constraints:__

- `1 <= num.length <= 10 ^ 5`
- `num` consists of digits.

__题目描述:__

给你一个仅由数字（ `0 - 9`）组成的字符串 `num` 。

请你找出能够使用 `num` 中数字形成的 __最大回文__ 整数，并以字符串形式返回。该整数不含 __前导零__ 。

注意：

- 你 __无需__ 使用 `num` 中的所有数字，但你必须使用 __至少__ 一个数字。
- 数字可以重新排序。

__示例:__

示例 1：

```text
输入：num = "444947137"
输出："7449447"
解释：
从 "444947137" 中选用数字 "4449477"，可以形成回文整数 "7449447" 。
可以证明 "7449447" 是能够形成的最大回文整数。
```

示例 2：

```text
输入：num = "00009"
输出："9"
解释：
可以证明 "9" 能够形成的最大回文整数。
注意返回的整数不应含前导零。
```

__提示：__

- `1 <= num.length <= 10 ^ 5`
- `num` 由数字（ `0 - 9`）组成

__思路:__

```text
贪心
统计每个数字出现的次数
如果只有一个数字 0, 则返回 "0"
否则, 从 9 到 1 遍历, 每次取出数字出现的一半次数, 作为回文数的前半部分
如果前半部分不为空, 则将 0 出现的一半次数作为回文数的中间部分
记录前半部分, 并将其反转得到后半部分
此时还可以加一个数字作为回文数的中间部分, 从 9 到 0 遍历, 如果数字出现次数为奇数, 则加入回文数的中间部分
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string largestPalindromic(string num) 
    {
        string front;
        vector<int> c(10);
        for (const auto& d : num) ++c[d - '0'];
        if (c[0] == num.size()) return "0";
        for (int i = 9; i > 0; i--) front += string(c[i] >> 1, '0' + i);
        if (!front.empty()) front += string(c[0] >> 1, '0');;
        string back(front.rbegin(), front.rend());
        for (int i = 9; i > -1; i--) 
        {
            if (c[i] & 1) 
            {
                front += to_string(i);
                break;
            }
        }
        return front + back;
    }
};
```

__Java__:

```Java
class Solution {
    public String largestPalindromic(String num) {
        StringBuilder front = new StringBuilder();
        int c[] = new int[10], n = num.length();
        for (char d : num.toCharArray()) ++c[d - '0'];
        if (c[0] == n) return "0";
        for (int i = 9; i > 0; i--) for (int j = 0; j < c[i] >> 1; j++) front.append((char)(i + '0'));
        if (!front.isEmpty()) for (int j = 0; j < c[0] >> 1; j++) front.append((char)('0'));
        StringBuilder back = new StringBuilder(front).reverse();
        for (int i = 9; i > -1; i--) {
            if ((c[i] & 1) == 1) {
                front.append((char)(i + '0'));
                break;
            }
        }
        return front.toString() + back.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def largestPalindromic(self, num: str) -> str:
        if len(c := Counter(num)) == 1 and '0' in c:
            return '0'
        front = ''
        for d in digits[:0:-1]:
            front += d * (c[d] >> 1)
        if front:
            front += '0' * (c['0'] >> 1)
        back = front[::-1]
        for d in digits[::-1]:
            if c[d] & 1:
                front += d
                break
        return front + back
```
