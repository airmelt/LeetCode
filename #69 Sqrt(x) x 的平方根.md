__Description__:
Implement int sqrt(int x).

Compute and return the square root of x, where x is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

__Example__:
Example 1:
Input: 4
Output: 2

Example 2:
Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since
             the decimal part is truncated, 2 is returned.

__题目描述__:
实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

 __示例__:
示例 1:
输入: 4
输出: 2

示例 2:
输入: 8
输出: 2
说明: 8 的平方根是 2.82842...,
     由于返回类型是整数，小数部分将被舍去。

__思路__:
1. 二分法, int最大值为2^31 - 1, 其平方根为46340
时间复杂度O(lgn), 空间复杂度O(1)
2. [牛顿法-wiki](https://en.wikipedia.org/wiki/Integer_square_root#Using_only_integer_division)
原理类似基本不等式: a^2 + b^2 >= 2ab
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int mySqrt(int x) {
        if (x <= 1) return x;
        int low = 0;
        int high = 46340;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (mid * mid > x) high = mid - 1;
            else if (mid * mid < x) low = mid + 1;
            else return mid;
        }
        return low - 1;
    }
};
```

__Java__:
```
class Solution {
    public int mySqrt(int x) {
        if (x <= 1) return x;
        int low = 0;
        int high = 46340;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (mid * mid > x) high = mid - 1;
            else if (mid * mid < x) low = mid + 1;
            else return mid;
        }
        return low - 1;
    }
}
```

__Python__:
```
class Solution:
    def mySqrt(self, x: int) -> int:
        if x <= 1:
            return x;
        result = x;
        while result > x / result:
            result = (result + x / result) // 2
        return int(result)
```
