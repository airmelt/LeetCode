# 1404 Number of Steps to Reduce a Number in Binary Representation to One 将二进制表示减到1的步骤数

__Description:__

Given the binary representation of an integer as a string s, return the number of steps to reduce it to 1 under the following rules:

If the current number is even, you have to divide it by 2.
If the current number is odd, you have to add 1 to it.

If the current number is even, you have to divide it by 2.

If the current number is odd, you have to add 1 to it.

It is guaranteed that you can always reach one for all test cases.

__Example:__

Example 1:

Input: s = "1101"
Output: 6
Explanation: "1101" corressponds to number 13 in their decimal representation.
Step 1) 13 is odd, add 1 and obtain 14.
Step 2) 14 is even, divide by 2 and obtain 7.
Step 3) 7 is odd, add 1 and obtain 8.
Step 4) 8 is even, divide by 2 and obtain 4.  
Step 5) 4 is even, divide by 2 and obtain 2.
Step 6) 2 is even, divide by 2 and obtain 1.

Example 2:

Input: s = "10"
Output: 1
Explanation: "10" corressponds to number 2 in their decimal representation.
Step 1) 2 is even, divide by 2 and obtain 1.

Example 3:

Input: s = "1"
Output: 0

__Constraints:__

1 <= s.length <= 500
s consists of characters '0' or '1'
s[0] == '1'

__题目描述:__

给你一个以二进制形式表示的数字 s 。请你返回按下述规则将其减少到 1 所需要的步骤数：

如果当前数字为偶数，则将其除以 2 。
如果当前数字为奇数，则将其加上 1 。

如果当前数字为偶数，则将其除以 2 。

如果当前数字为奇数，则将其加上 1 。

题目保证你总是可以按上述规则将测试用例变为 1 。

__示例:__

示例 1：

输入：s = "1101"
输出：6
解释："1101" 表示十进制数 13 。
Step 1) 13 是奇数，加 1 得到 14
Step 2) 14 是偶数，除 2 得到 7
Step 3) 7  是奇数，加 1 得到 8
Step 4) 8  是偶数，除 2 得到 4  
Step 5) 4  是偶数，除 2 得到 2
Step 6) 2  是偶数，除 2 得到 1

示例 2：

输入：s = "10"
输出：1
解释："10" 表示十进制数 2 。
Step 1) 2 是偶数，除 2 得到 1

示例 3：

输入：s = "1"
输出：0

__提示：__

1 <= s.length <= 500
s 由字符 '0' 或 '1' 组成。
s[0] == '1'

__思路:__

```text
模拟
如果字符串中只有 1 个 '1' 说明字符串是 '10000...' 形式
这种情况只要一直除 2 即可, 返回 len(s) - 1
否则对于第一个 '1' 和最后一个 '1' 中间的所有 '0' 都需要变成 '1', 记为 zero
每个 '0' 变成 '1' 都需要 1 步操作, 实际上可以将除 2 的操作放到最后统一执行, 所以把任意 1 个中间的 '0' 变成 '1' 只需要操作 1 步
最后会变成 '10000...' 的形式
还需要 len(s) 步操作
比如 '11010' -> '11110' -> '100000' -> '1'
需要返回 zero + len(s) + 1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numSteps(string s) 
    {
        int zero = 0, n = s.size(), i = n - 1;
        while (i >= 0 && s[i] != '1') --i;
        if (i == 0) return n - 1;
        while (i > 0) if (s[i--] == '0') ++zero;
        return zero + n + 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int numSteps(String s) {
        int zero = 0, n = s.length(), i = n - 1;
        while (i >= 0 && s.charAt(i) != '1') --i;
        if (i == 0) return n - 1;
        while (i > 0) if (s.charAt(i--) == '0') ++zero;
        return zero + n + 1;
    }
}
```

__Python__:

```Python
class Solution:
    def numSteps(self, s: str) -> int:
        return len(s) - 1 if (one := s.count('1')) == 1 else (zero := s[s.find('1') + 1:s.rfind('1')].count('0')) + len(s) + 1
```
