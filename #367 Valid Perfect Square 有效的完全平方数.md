__Description__:
Given a positive integer num, write a function which returns True if num is a perfect square else False.

__Note:__ Do not use any built-in library function such as sqrt.

**Example :**
Example 1:
Input: 16
Output: true

Example 2:
Input: 14
Output: false

__题目描述__:
给定一个正整数 num，编写一个函数，如果 num 是一个完全平方数，则返回 True，否则返回 False。

__说明:__ 不要使用任何内置的库函数，如  sqrt。

**示例 :**
示例 1：
输入：16
输出：True

示例 2：
输入：14
输出：False

__思路__:
1. 利用正奇数之和等于平方数 1 + 3 + ... + (2n - 1) = n ^ 2
2. 二分查找
最大的平方数为 46340 ^ 2 < 2 ^ 31 - 1
时间复杂度O(logn), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    bool isPerfectSquare(int num) {
        int temp = 1;
        while (num > 0) {
            num -= temp;
            temp += 2;
        }
        return num == 0;
    }
};
```

__Java__:
```
class Solution {
    public boolean isPerfectSquare(int num) {
        int low = 0;
        int high = num > 46340 ? 46340 : num;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (num == mid * mid) return true;
            else if (num > mid * mid) low = mid + 1;
            else high = mid - 1;
        }
        return false;
    }
}
```

__Python__:
```
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        return (num in [i ** 2 for i in range(46341)])
```
