# 191 Number of 1 Bits 位1的个数

__Description__:
Write a function that takes an unsigned integer and return the number of '1' bits it has (also known as the [Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).

**Example:**

**Example 1:**
**Input:** 00000000000000000000000000001011
**Output:** 3
**Explanation:** The input binary string **00000000000000000000000000001011** has a total of three '1' bits.

**Example 2:**
**Input:** 00000000000000000000000010000000
**Output:** 1
**Explanation:** The input binary string **00000000000000000000000010000000** has a total of one '1' bit.

**Example 3:**
**Input:** 11111111111111111111111111111101
**Output:** 31
**Explanation:** The input binary string **11111111111111111111111111111101** has a total of thirty one '1' bits.

**Note:**

* Note that in some languages such as Java, there is no unsigned integer type. In this case, the input will be given as signed integer type and should not affect your implementation, as the internal binary representation of the integer is the same whether it is signed or unsigned.
* In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two%27s_complement). Therefore, in **Example 3** above the input represents the signed integer `-3`.

**Follow up**:

If this function is called many times, how would you optimize it?

__题目描述__:
编写一个函数，输入是一个无符号整数，返回其二进制表达式中数字位数为 ‘1’ 的个数（也被称为[汉明重量](https://baike.baidu.com/item/%E6%B1%89%E6%98%8E%E9%87%8D%E9%87%8F)）。

**示例：**

**示例 1：**
**输入：** 00000000000000000000000000001011
**输出：** 3
**解释：** 输入的二进制串 **00000000000000000000000000001011** 中，共有三位为 '1'。

**示例 2：**
**输入：** 00000000000000000000000010000000
**输出：** 1
**解释：** 输入的二进制串 **00000000000000000000000010000000** 中，共有一位为 '1'。

**示例 3：**
**输入：** 11111111111111111111111111111101
**输出：** 31
**解释：** 输入的二进制串 **11111111111111111111111111111101** 中，共有 31 位为 '1'。

**提示：**

* 请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
* 在 Java 中，编译器使用[二进制补码](https://baike.baidu.com/item/%E4%BA%8C%E8%BF%9B%E5%88%B6%E8%A1%A5%E7%A0%81/5295284)记法来表示有符号整数。因此，在上面的 **示例 3** 中，输入表示有符号整数 `-3`。

**进阶**:
如果多次调用这个函数，你将如何优化你的算法？

__思路__:

参考[LeetCode #136 Single Number 只出现一次的数字](https://www.jianshu.com/p/d8050ac9d91d)

1. n & (n - 1) 统计1出现的次数
2. n & 1统计1出现的次数
3. 最直接的方法是 n % 2 统计1出现的次数

3种方法的C++测试时间和内存消耗差别不大
时间复杂度O(1), 空间复杂度O(1), 由于 n最多为32位二进制数, 总是能在O(1)内完成
推广到 n位二进制数, 时间复杂度为O(lgn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int hammingWeight(uint32_t n) 
    {
        int result = 0;
        while (n) 
        {
            result += (n & 1);
            n >>= 1;
        }
        return result;
    }
};
```

__Java__:

```Java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int result = 0;
        for(int i = 0; i < 32; i++) {
            if (((n >> i) & 1) == 1) result++;
        }    
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def hammingWeight(self, n: int) -> int:
        return bin(n).count('1')
```
