# 2864 Maximum Odd Binary Number 最大二进制奇数

__Description:__

You are given a __binary__ string `s` that contains at least one `'1'`.

You have to rearrange the bits in such a way that the resulting binary number is the maximum odd binary number that can be created from this combination.

Return a string representing the maximum odd binary number that can be created from the given combination.

Note that the resulting string can have leading zeros.

__Example:__

Example 1:

```text
Input: s = "010"
Output: "001"
Explanation: Because there is just one '1', it must be in the last position. So the answer is "001".
```

Example 2:

```text
Input: s = "0101"
Output: "1001"
Explanation: One of the '1's must be in the last position. The maximum number that can be made with the remaining digits is "100". So the answer is "1001".
```

__Constraints:__

- `1 <= s.length <= 100`
- `s` consists only of `'0'` and `'1'`.
- `s` contains at least one `'1'`.

__题目描述:__

给你一个 __二进制__ 字符串 `s` ，其中至少包含一个 `'1'` 。

你必须按某种方式 重新排列 字符串中的位，使得到的二进制数字是可以由该组合生成的 最大二进制奇数 。

以字符串形式，表示并返回可以由给定组合生成的最大二进制奇数。

注意 返回的结果字符串 可以 含前导零。

__示例:__

示例 1：

```text
输入：s = "010"
输出："001"
解释：因为字符串 s 中仅有一个 '1' ，其必须出现在最后一位上。所以答案是 "001" 。
```

示例 2：

```text
输入：s = "0101"
输出："1001"
解释：其中一个 '1' 必须出现在最后一位上。而由剩下的数字可以生产的最大数字是 "100" 。所以答案是 "1001" 。
```

__提示：__

- `1 <= s.length <= 100`
- `s` 仅由 `'0'` 和 `'1'` 组成
- `s` 中至少包含一个 `'1'`

__思路:__

```text
模拟
要保证奇数只需要最低位为 '1'
要使得结果最大尽可能往高位放 '1'
所以结果为 '1' 的个数减去 1 + '0' 的个数 + '1'
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string maximumOddBinaryNumber(string s) 
    {
        return string(ranges::count(s, '1') - 1, '1') + string(s.size() - ranges::count(s, '1'), '0') + '1';
    }
};
```

__Java__:

```Java
class Solution {
    public String maximumOddBinaryNumber(String s) {
        return "1".repeat((int)s.chars().filter(c -> c == '1').count() - 1) + "0".repeat(s.length() - (int)s.chars().filter(c -> c == '1').count()) + "1";
    }
}
```

__Python__:

```Python
class Solution:
    def maximumOddBinaryNumber(self, s: str) -> str:
        return '1' * (s.count('1') - 1) + '0' * s.count('0') + '1'
```
