# 371 Sum of Two Integers 两整数之和

__Description__:
Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.

**Example :**

Example 1:
Input: a = 1, b = 2
Output: 3

Example 2:
Input: a = -2, b = 3
Output: 1

__题目描述__:
不使用运算符 + 和 - ​​​​​​​，计算两整数 ​​​​​​​a 、b ​​​​​​​之和。

**示例 :**

示例 1:
输入: a = 1, b = 2
输出: 3

示例 2:
输入: a = -2, b = 3
输出: 1

__思路__:

计算加法的方法: 将两个数用补码表示, 每一位进行异或, 每一位进行与可以得到进位
LeetCode编辑器不能进行负数的移位, 所以要用 & 0xffffffff来使负数可以移位
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int getSum(int a, int b) 
    {
        return b == 0 ? a : getSum(a ^ b, ((a & b) & 0xffffffff) << 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int getSum(int a, int b) {
        while (b != 0) {
            int temp = a ^ b;
            b = (a & b) << 1;
            a = temp;
        }
        return a;
    }
}
```

__Python__:

```Python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        while b:
            a, b = (a ^ b) & 0xffffffff, (a & b) << 1
        return a if a <= 0x7fffffff else ~(a ^ 0xffffffff)
```
