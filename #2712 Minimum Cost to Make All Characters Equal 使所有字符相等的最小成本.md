# 2712 Minimum Cost to Make All Characters Equal 使所有字符相等的最小成本

__Description:__

You are given a __0-indexed__ binary string `s` of length `n` on which you can apply two types of operations:

- Choose an index `i` and invert all characters from index `0` to index `i` (both inclusive), with a cost of `i + 1`
- Choose an index `i` and invert all characters from index `i` to index `n - 1` (both inclusive), with a cost of `n - i`

Return the minimum cost to make all characters of the string equal.

Invert a character means if its value is '0' it becomes '1' and vice-versa.

__Example:__

Example 1:

```text
Input:  s = "0011"
Output:  2
Explanation:  Apply the second operation with i = 2 to obtain s = "0000" for a cost of 2. It can be shown that 2 is the minimum cost to make all characters equal.
```

Example 2:

```text
Input: s = "010101"
Output: 9
Explanation: Apply the first operation with i = 2 to obtain s = "101101" for a cost of 3.
Apply the first operation with i = 1 to obtain s = "011101" for a cost of 2. 
Apply the first operation with i = 0 to obtain s = "111101" for a cost of 1. 
Apply the second operation with i = 4 to obtain s = "111110" for a cost of 2.
Apply the second operation with i = 5 to obtain s = "111111" for a cost of 1. 
The total cost to make all characters equal is 9. It can be shown that 9 is the minimum cost to make all characters equal.
```

__Constraints:__

- `1 <= s.length == n <= 10 ^ 5`
- `s[i]` is either `'0'` or `'1'`

__题目描述:__

给你一个下标从 __0__ 开始、长度为 `n` 的二进制字符串 `s` ，你可以对其执行两种操作:

- 选中一个下标 `i` 并且反转从下标 `0` 到下标 `i`（包括下标 `0` 和下标 `i` ）的所有字符，成本为 `i + 1` 。
- 选中一个下标 `i` 并且反转从下标 `i` 到下标 `n - 1`（包括下标 `i` 和下标 `n - 1` ）的所有字符，成本为 `n - i` 。

返回使字符串内所有字符 相等 需要的 最小成本 。

反转 字符意味着：如果原来的值是 '0' ，则反转后值变为 '1' ，反之亦然。

__示例:__

示例 1：

```text
输入: s = "0011"
输出: 2
解释: 执行第二种操作，选中下标 i = 2 ，可以得到 s = "0000" ，成本为 2。可以证明 2 是使所有字符相等的最小成本。
```

示例 2：

```text
输入：s = "010101"
输出：9
解释：执行第一种操作，选中下标 i = 2 ，可以得到 s = "101101" ，成本为 3 。
执行第一种操作，选中下标 i = 1 ，可以得到 s = "011101" ，成本为 2 。
执行第一种操作，选中下标 i = 0 ，可以得到 s = "111101" ，成本为 1 。
执行第二种操作，选中下标 i = 4 ，可以得到 s = "111110" ，成本为 2 。
执行第二种操作，选中下标 i = 5 ，可以得到 s = "111111" ，成本为 1 。
使所有字符相等的总成本等于 9 。可以证明 9 是使所有字符相等的最小成本。
```

__提示：__

- `1 <= s.length == n <= 10 ^ 5`
- `s[i]` 为 `'0'` 或 `'1'`

__思路:__

```text
贪心
每次反转要么把前面的所有字符都反转, 要么把后面的字符都反转
那么将不会改变两个相邻字符的相对值
所以只要遇到两个相邻字符不相等就必须反转
取到两端距离的较小值就是代价
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumCost(string s) 
    {
        long long result = 0LL;
        for (int i = 1, n = s.size(); i < n; i++) if (s[i - 1] != s[i]) result += min(i, n - i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumCost(String s) {
        long result = 0L;
        for (int i = 1, n = s.length(); i < n; i++) if (s.charAt(i - 1) != s.charAt(i)) result += Math.min(i, n - i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumCost(self, s: str) -> int:
        return sum(min(i, len(s) - i) for i in range(1, len(s)) if s[i] != s[i - 1])
```
