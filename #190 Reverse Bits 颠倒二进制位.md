__Description__:
Reverse bits of a given 32 bits unsigned integer.

**Example 1:**
**Input:** 00000010100101000001111010011100
**Output:** 00111001011110000010100101000000
**Explanation:** The input binary string **00000010100101000001111010011100** represents the unsigned integer 43261596, so return 964176192 which its binary representation is **00111001011110000010100101000000**.

**Example 2:**
**Input:** 11111111111111111111111111111101
**Output:** 10111111111111111111111111111111
**Explanation:** The input binary string **11111111111111111111111111111101** represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is **10101111110010110010011101101001**.

**Note:**

*   Note that in some languages such as Java, there is no unsigned integer type. In this case, both input and output will be given as signed integer type and should not affect your implementation, as the internal binary representation of the integer is the same whether it is signed or unsigned.
*   In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two%27s_complement). Therefore, in **Example 2** above the input represents the signed integer `-3` and the output represents the signed integer `-1073741825`.

**Follow up**:

If this function is called many times, how would you optimize it?

__题目描述__:
颠倒给定的 32 位无符号整数的二进制位。

**示例 1：**
**输入:** 00000010100101000001111010011100
**输出:** 00111001011110000010100101000000
**解释:** 输入的二进制串 **00000010100101000001111010011100** 表示无符号整数 **43261596****，
**      因此返回 964176192，其二进制表示形式为 **00111001011110000010100101000000**。

**示例 2：**
**输入：**11111111111111111111111111111101
**输出：**10111111111111111111111111111111
**解释：**输入的二进制串 **11111111111111111111111111111101** 表示无符号整数 4294967293，
      因此返回 3221225471 其二进制表示形式为 **10101111110010110010011101101001。**

**提示：**

*   请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
*   在 Java 中，编译器使用[二进制补码](https://baike.baidu.com/item/%E4%BA%8C%E8%BF%9B%E5%88%B6%E8%A1%A5%E7%A0%81/5295284)记法来表示有符号整数。因此，在上面的 **示例 2** 中，输入表示有符号整数 `-3`，输出表示有符号整数 `-1073741825`。

**进阶**:
如果多次调用这个函数，你将如何优化你的算法？

__思路__:
考虑将输入从最后一位开始逐位插入到结果中, 使用位运算可以最大化效率
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        // uint32_t 8字节无符号整数
        uint32_t result = 0;
        // 32bit
        int i = 32;
        while (i--) {
            // result 左移 1位, 存放下一个 n的最后一位
            result <<= 1;
            /* n与 1结果为 n的最后一位
             * 0 & 1 = 0;
             * 1 & 1 = 1;
             */
            result += n & 1;
            // n 右移 1位去掉最后一位的值
            n >>= 1;
        }
        return result;
    }
};
```

__Java__:
```
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int result = 0;
        int i = 32;
        while (i-- != 0) {
            result <<= 1;
            result += n & 1;
            n >>= 1;
        }
        return result;
    }
}
```

__Python__:
```
class Solution:
    def reverseBits(self, n: int) -> int:
        return int(bin(n)[-1:1:-1].ljust(32, '0'), 2)
```
