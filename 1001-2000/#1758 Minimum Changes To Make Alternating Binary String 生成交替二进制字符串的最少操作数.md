# 1758 Minimum Changes To Make Alternating Binary String 生成交替二进制字符串的最少操作数

__Description:__

You are given a string `s` consisting only of the characters `'0'` and `'1'`. In one operation, you can change any `'0'` to `'1'` or vice versa.

The string is called alternating if no two adjacent characters are equal. For example, the string `"010"` is alternating, while the string `"0100"` is not.

Return _the __minimum__ number of operations needed to make_ `s` _alternating_.

__Example:__

Example 1:

```text
Input: s = "0100"
Output: 1
Explanation: If you change the last character to '1', s will be "0101", which is alternating.
```

Example 2:

```text
Input: s = "10"
Output: 0
Explanation: s is already alternating.
```

Example 3:

```text
Input: s = "1111"
Output: 2
Explanation: You need two operations to reach "0101" or "1010".
```

__Constraints:__

- `1 <= s.length <= 10 ^ 4`
- `s[i]` is either `'0'` or `'1'`.

__题目描述:__

给你一个仅由字符 `'0'` 和 `'1'` 组成的字符串 `s` 。一步操作中，你可以将任一 `'0'` 变成 `'1'` ，或者将 `'1'` 变成 `'0'` 。

__交替字符串__ 定义为:如果字符串中不存在相邻两个字符相等的情况，那么该字符串就是交替字符串。例如，字符串 `"010"` 是交替字符串，而字符串 `"0100"` 不是。

返回使 `s` 变成 __交替字符串__ 所需的 __最少__ 操作数。

__示例:__

示例 1：

```text
输入：s = "0100"
输出：1
解释：如果将最后一个字符变为 '1' ，s 就变成 "0101" ，即符合交替字符串定义。
```

示例 2：

```text
输入：s = "10"
输出：0
解释：s 已经是交替字符串。
```

示例 3：

```text
输入：s = "1111"
输出：2
解释：需要 2 步操作得到 "0101" 或 "1010" 。
```

__提示：__

- `1 <= s.length <= 10 ^ 4`
- `s[i]` 是 `'0'` 或 `'1'`

__思路:__

```text
模拟
转化的结果要么为 '0101010' 要么为 '10101010'
比较较少的修改次数并返回即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(string s) 
    {
        int a = 0, n = s.size();
        for (int i = 0; i < n; i++) a += s[i] ^ '0' ^ (i & 1);
        return min(a, n - a);
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(String s) {
        int a = 0, n = s.length();
        char[] arr = s.toCharArray();
        for (int i = 0; i < n; i++) a += arr[i] ^ '0' ^ (i & 1);
        return Math.min(a, n - a);
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, s: str) -> int:
        return min(count, len(s) - count) if (count := sum(int(c) != i % 2 for i, c in enumerate(s))) else 0
```
