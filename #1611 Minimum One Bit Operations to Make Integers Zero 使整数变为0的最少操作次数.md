# 1611 Minimum One Bit Operations to Make Integers Zero 使整数变为0的最少操作次数

__Description:__

Given an integer `n`, you must transform it into `0` using the following operations any number of times:

- Change the rightmost ( `0 ^ th`) bit in the binary representation of `n`.
- Change the `i ^ th` bit in the binary representation of `n` if the `(i-1) ^ th` bit is set to `1` and the `(i-2) ^ th` through `0 ^ th` bits are set to `0`.

Return _the minimum number of operations to transform_ `n` _into_ `0`_._

__Example:__

Example 1:

```text
Input: n = 3
Output: 2
Explanation: The binary representation of 3 is "11".
"11" -> "01" with the 2nd operation since the 0th bit is 1.
"01" -> "00" with the 1st operation.
```

Example 2:

```text
Input: n = 6
Output: 4
Explanation: The binary representation of 6 is "110".
"110" -> "010" with the 2nd operation since the 1st bit is 1 and 0th through 0th bits are 0.
"010" -> "011" with the 1st operation.
"011" -> "001" with the 2nd operation since the 0th bit is 1.
"001" -> "000" with the 1st operation.
```

__Constraints:__

- `0 <= n <= 10 ^ 9`

__题目描述:__

给你一个整数 `n`，你需要重复执行多次下述操作将其转换为 `0` :

- 翻转 `n` 的二进制表示中最右侧位（第 `0` 位）。
- 如果第 `(i-1)` 位为 `1` 且从第 `(i-2)` 位到第 `0` 位都为 `0`，则翻转 `n` 的二进制表示中的第 `i` 位。

返回将 `n` 转换为 `0` 的最小操作次数。

__示例:__

示例 1：

```text
输入：n = 3
输出：2
解释：3 的二进制表示为 "11"
"11" -> "01" ，执行的是第 2 种操作，因为第 0 位为 1 。
"01" -> "00" ，执行的是第 1 种操作。
```

示例 2：

```text
输入：n = 6
输出：4
解释：6 的二进制表示为 "110".
"110" -> "010" ，执行的是第 2 种操作，因为第 1 位为 1 ，第 0 到 0 位为 0 。
"010" -> "011" ，执行的是第 1 种操作。
"011" -> "001" ，执行的是第 2 种操作，因为第 0 位为 1 。
"001" -> "000" ，执行的是第 1 种操作。
```

__提示：__

- `0 <= n <= 10 ^ 9`

__思路:__

[格雷码](https://oi-wiki.org/misc/gray-code/)

```text
格雷码
题目中的两个操作的定义即为格雷码
实际上就是求 n 在格雷码中的序号
根据格雷码为 n ^ (n >> 1) 求取格雷码的逆变换即可
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumOneBitOperations(int n) 
    {
        int result = 0;
        while (n)
        {
            result ^= n;
            n >>= 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumOneBitOperations(int n) {
        return n == 0 ? n : n ^ minimumOneBitOperations(n >> 1);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumOneBitOperations(self, n: int) -> int:
        return n ^ self.minimumOneBitOperations(n >> 1) if n else n
```
