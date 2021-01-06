# 397 Integer Replacement 整数替换

__Description__:
Given a positive integer n, you can apply one of the following operations:

If n is even, replace n with n / 2.
If n is odd, replace n with either n + 1 or n - 1.
Return the minimum number of operations needed for n to become 1.

__Example:__

Example 1:

Input: n = 8
Output: 3
Explanation: 8 -> 4 -> 2 -> 1

Example 2:

Input: n = 7
Output: 4
Explanation: 7 -> 8 -> 4 -> 2 -> 1
or 7 -> 6 -> 3 -> 2 -> 1

Example 3:

Input: n = 4
Output: 2

__Constraints:__

1 <= n <= 2^31 - 1

__题目描述__:
给定一个正整数 n ，你可以做如下操作：

如果 n 是偶数，则用 n / 2替换 n 。
如果 n 是奇数，则可以用 n + 1或n - 1替换 n 。
n 变为 1 所需的最小替换次数是多少？

__示例 :__

示例 1：

输入：n = 8
输出：3
解释：8 -> 4 -> 2 -> 1

示例 2：

输入：n = 7
输出：4
解释：7 -> 8 -> 4 -> 2 -> 1
或 7 -> 6 -> 3 -> 2 -> 1

示例 3：

输入：n = 4
输出：2

__提示：__

1 <= n <= 2^31 - 1

__思路__:

数学法
偶数直接除 2
奇数分为 0bxxx01和 0bxxx11和 3讨论
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int integerReplacement(int n) 
    {
        int result = 0;
        while (n > 1)
        {
            if (!(n & 1) or n == INT_MAX) n >>= 1;
            else if ((n & 2) == 2 and n > 3) ++n;
            else --n;
            ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int integerReplacement(int n) {
        int result = 0;
        while (n != 1) {
            if ((n & 1) == 0) n >>>= 1;
            else if ((n & 2) == 2 && n > 3) ++n;
            else --n;
            ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def integerReplacement(self, n: int) -> int:
        result = 0
        while n != 1:
            if not (n & 1):
                n >>= 1
            else:
                n += -1 if (n & 2) == 0 or n == 3 else 1
            result += 1
        return result
```
