# 231 Power of Two 2的幂

__Description__:
Given an integer, write a function to determine if it is a power of two.

**Example:**

Example 1:
Input: 1
Output: true
Explanation: 2^0 = 1

Example 2:
Input: 16
Output: true
Explanation: 2^4 = 16

Example 3:
Input: 218
Output: false

__题目描述__:
给定一个整数，编写一个函数来判断它是否是 2 的幂次方。

**示例：**

示例 1:
输入: 1
输出: true
解释: 2^0 = 1

示例 2:
输入: 16
输出: true
解释: 2^4 = 16

示例 3:
输入: 218
输出: false

__思路__:

位操作

1. 2的幂有且仅有 1位是1 n & (n - 1) 可以用来去掉最后一位的 1
2. 如果 n是 2的幂, n & (-n) == n成立, -n的补码只有最后一位跟 n相同且均为 1
补码为正数的所有位取反再加 1
不妨设 n用8位存储, n = 16 -> n = 00010000, -n = -16 -> -n = 11110000, n & -n = n
3. 如果 n是 2的幂, 那么 n一定能被 2 ^ 30整除(int最大为 2 ^ 31 - 1)
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        return n > 0 and (1 << 31) % n == 0;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && (n & n - 1) == 0;
    }
}
```

__Python__:

```Python
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        return n > 0 and -n & n == n
```
