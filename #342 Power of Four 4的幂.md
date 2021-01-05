# 342 Power of Four 4的幂

__Description__:
Given an integer (signed 32 bits), write a function to check whether it is a power of 4.

**Example :**

Example 1:
Input: 16
Output: true

Example 2:
Input: 5
Output: false

__Follow up:__
Could you do it without using any loop / recursion?

__题目描述__:
给定一个整数 (32 位有符号整数)，请编写一个函数来判断它是否是 4 的幂次方。

**示例 :**

示例 1:
输入: 16
输出: true

示例 2:
输入: 5
输出: false

__进阶：__
你能不使用循环或者递归来完成本题吗？

__思路__:

1. 循环判断 n % 4 == 0, 执行 n /= 4直到 n == 1
2. 递归, 方法同 1
3. 4的幂的特点: 转换成二进制后, 仅有一位是1, 并且这一位一定在奇数位
如16 -> 0b10000, 1 -> 0b1
判断仅有一位是 1可以用 num & (num - 1) == 0
判断是否在奇数位可以用 num & 0xaaaaaaaa, 其中 a为 0b1010
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isPowerOfFour(int num) 
    {
        return num > 0 and ((num & (num - 1)) == 0) && ((num & 0xaaaaaaaa) == 0);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isPowerOfFour(int num) {
        return num > 0 && ((num & (num - 1)) == 0) && ((num & 0xaaaaaaaa) == 0);
    }
}
```

__Python__:

```Python
class Solution:
    def isPowerOfFour(self, num: int) -> bool:
        return num > 0 and not num & (num - 1) and not num & 0xaaaaaaaa
```
