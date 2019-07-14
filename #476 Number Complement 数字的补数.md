__Description__:
Given a positive integer, output its complement number. The complement strategy is to flip the bits of its binary representation.

__Note:__
The given integer is guaranteed to fit within the range of a 32-bit signed integer.
You could assume no leading zero bit in the integer’s binary representation.

__Example:__
Example 1:
Input: 5
Output: 2
Explanation: The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.

Example 2:
Input: 1
Output: 0
Explanation: The binary representation of 1 is 1 (no leading zero bits), and its complement is 0. So you need to output 0.

__题目描述__:
给定一个正整数，输出它的补数。补数是对该数的二进制表示取反。

__注意:__

给定的整数保证在32位带符号整数的范围内。
你可以假定二进制数不包含前导零位。

__示例 :__
示例 1:

输入: 5
输出: 2
解释: 5的二进制表示为101（没有前导零位），其补数为010。所以你需要输出2。

示例 2:

输入: 1
输出: 0
解释: 1的二进制表示为1（没有前导零位），其补数为0。所以你需要输出0。

__思路__:
找到一个最小的大于 num的 2 ^ i - 1的正整数, 返回 2 ^ i - 1 - num即可
实质上是取到一个mask(全1), 返回 mask ^ num
e.g. 5 -> 0x101 -> mask = 2 ^ 3 - 1 = 0x111 -> return 010
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int findComplement(int num) {
        int i = 1;
        while (i < num) {
            i <<= 1;
            i++;
        }
        return (i ^ num);
    }
};
```

__Java__:
```
class Solution {
    public int findComplement(int num) {
        for (int i = 0; i < 32; i++) if (num < (1 << (i + 1))) return (1 << (i + 1)) - 1 - num;
        return Integer.MAX_VALUE - num;
    }
}
```

__Python__:
```
class Solution:
    def findComplement(self, num: int) -> int:
        return num ^ int((len(bin(num)) - 2) * '1', 2)
```
