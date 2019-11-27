__Description__:
Every non-negative integer N has a binary representation.  For example, 5 can be represented as "101" in binary, 11 as "1011" in binary, and so on.  Note that except for N = 0, there are no leading zeroes in any binary representation.

The complement of a binary representation is the number in binary you get when changing every 1 to a 0 and 0 to a 1.  For example, the complement of "101" in binary is "010" in binary.

For a given number N in base-10, return the complement of it's binary representation as a base-10 integer.

__Example:__
Example 1:

Input: 5
Output: 2
Explanation: 5 is "101" in binary, with complement "010" in binary, which is 2 in base-10.

Example 2:

Input: 7
Output: 0
Explanation: 7 is "111" in binary, with complement "000" in binary, which is 0 in base-10.

Example 3:

Input: 10
Output: 5
Explanation: 10 is "1010" in binary, with complement "0101" in binary, which is 5 in base-10.
 
__Note:__

0 <= N < 10^9

__题目描述__:
每个非负整数 N 都有其二进制表示。例如， 5 可以被表示为二进制 "101"，11 可以用二进制 "1011" 表示，依此类推。注意，除 N = 0 外，任何二进制表示中都不含前导零。

二进制的反码表示是将每个 1 改为 0 且每个 0 变为 1。例如，二进制数 "101" 的二进制反码为 "010"。

给定十进制数 N，返回其二进制表示的反码所对应的十进制整数。

__示例 :__
示例 1：

输入：5
输出：2
解释：5 的二进制表示为 "101"，其二进制反码为 "010"，也就是十进制中的 2 。

示例 2：

输入：7
输出：0
解释：7 的二进制表示为 "111"，其二进制反码为 "000"，也就是十进制中的 0 。

示例 3：

输入：10
输出：5
解释：10 的二进制表示为 "1010"，其二进制反码为 "0101"，也就是十进制中的 5 。
 
__提示：__

0 <= N < 10^9

__思路__:
这里的取反和定义不太一样, 只用取到大于或等于 N的 2 ^ k即可
1. 输出的结果为 2 ^ k - 1 - N, 因为 N + result = 2 ^ k - 1
也可以是 N ^ (2 ^ k - 1), 第一个 ^ 为位运算操作符
2. k = log(2)(N) = ln(N) / ln(2)
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int bitwiseComplement(int N) 
    {
        return N ? N ^ (2 << (int)(log(N) / log(2))) - 1 : 1;
    }
};
```

__Java__:
```Java
class Solution {
    public int bitwiseComplement(int N) {
        return (int)Math.pow(2, Integer.toBinaryString(N).length()) - 1 ^ N;
    }
}
```

__Python__:
```Python
class Solution:
    def bitwiseComplement(self, N: int) -> int:
        return N ^ (2 ** (len(bin(N)) - 2) - 1)
```