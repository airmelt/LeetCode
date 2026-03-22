# 1017 Convert to Base -2 负二进制转换

__Description__:
Given an integer n, return a binary string representing its representation in base -2.

Note that the returned string should not have leading zeros unless the string is "0".

__Example:__

Example 1:

Input: n = 2
Output: "110"
Explantion: (-2)2 + (-2)1 = 2

Example 2:

Input: n = 3
Output: "111"
Explantion: (-2)2 + (-2)1 + (-2)0 = 3

Example 3:

Input: n = 4
Output: "100"
Explantion: (-2)2 = 4

__Constraints:__

0 <= n <= 10^9

__题目描述__:
给出数字 N，返回由若干 "0" 和 "1"组成的字符串，该字符串为 N 的负二进制（base -2）表示。

除非字符串就是 "0"，否则返回的字符串中不能含有前导零。

__示例 :__

示例 1：

输入：2
输出："110"
解释：(-2) ^ 2 + (-2) ^ 1 = 2

示例 2：

输入：3
输出："111"
解释：(-2) ^ 2 + (-2) ^ 1 + (-2) ^ 0 = 3

示例 3：

输入：4
输出："100"
解释：(-2) ^ 2 = 4

__提示:__

0 <= N <= 10^9

__思路__:

模拟
按照 2 进制转换
不过每次都需要变号
时间复杂度为 O(lgn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string baseNeg2(int n)
    {
        string result;
        while (n)
        {
            result += to_string(n & 1);
            n = -(n >> 1);
        }
        reverse(result.begin(), result.end());
        return result.empty() ? "0" : result;
    }
};
```

__Java__:

```Java
class Solution {
    public String baseNeg2(int n) {
        StringBuilder result = new StringBuilder();
        while (n != 0) {
            result.append(n & 1);
            n = -(n >> 1);
        }
        return result.isEmpty() ? "0" : result.reverse().toString();
    }
}
```

__Python__:

```Python
class Solution:
    def baseNeg2(self, n: int) -> str:
        result = ''
        while n:
            n, k = -(n >> 1), n & 1
            result += str(k)
        return result[::-1] if result else '0'
```
