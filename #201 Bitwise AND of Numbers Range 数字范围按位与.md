__Description__:
Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.

__Example:__
Example 1:

Input: [5,7]
Output: 4

Example 2:

Input: [0,1]
Output: 0

__题目描述__:
给定范围 [m, n]，其中 0 <= m <= n <= 2147483647，返回此范围内所有数字的按位与（包含 m, n 两端点）。

__示例 :__
示例 1: 

输入: [5,7]
输出: 4

示例 2:

输入: [0,1]
输出: 0

__思路__:
由于要返回所有数字的按位与
只要范围内有一位为 0, 则结果的该位为 0
所以要输出 m和 n的公共前缀
对 n操作, 去掉 n最右边的 1直到 n <= m, 返回 n
这样就可以获得 m和n的共同前缀
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int rangeBitwiseAnd(int m, int n) 
    {
        while (m < n) n &= n - 1;
        return n;
    }
};
```

__Java__:
```Java
class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        while (m < n) n &= n - 1;
        return n;
    }
}
```

__Python__:
```Python
class Solution:
    def rangeBitwiseAnd(self, m: int, n: int) -> int:
        while m < n:
            n &= n - 1
        return n
```