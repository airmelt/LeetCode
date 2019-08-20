__Description__:
Given a non-negative integer c, your task is to decide whether there're two integers a and b such that a^2 + b^2 = c.

__Example:__
Example 1:

Input: 5
Output: True
Explanation: 1 * 1 + 2 * 2 = 5
 
Example 2:

Input: 3
Output: False

__题目描述__:
给定一个非负整数 c ，你要判断是否存在两个整数 a 和 b，使得 a2 + b2 = c。

__示例 :__
示例1:

输入: 5
输出: True
解释: 1 * 1 + 2 * 2 = 5

示例2:

输入: 3
输出: False

__思路__:
1. 即求 x ^ 2 + y ^ 2 = r ^ 2的正整数解, 图形为一在第一象限的圆弧, 又因为该图形关于 x = y直线对称, 取 c / 2的开方判断是否有正整数解即可
2. 可以用双指针, 小于 c时左指针增加, 大于 c时右指针减少
时间复杂度O(n ^ 1 / 2), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    bool judgeSquareSum(int c) {
        int b = sqrt(c / 2);
        for (int a = 0; a <= b; a++) {
            int n = c - a * a;
            if (((int)sqrt(n) * (int)sqrt(n)) == n) return true;
        }
        return false;
    }
};
```

__Java__:
```
class Solution {
    public boolean judgeSquareSum(int c) {
        int b = (int)Math.sqrt(c / 2);
        for (int a = 0; a <= b; a++) {
            int n = c - a * a;
            if (((int)Math.sqrt(n) * (int)Math.sqrt(n)) == n) return true;
        }
        return false;
    }
}
```

__Python__:
```
class Solution:
    def judgeSquareSum(self, c: int) -> bool:
        i, j = 0, int(math.sqrt(c))
        while i <= j:
            temp = i ** 2 + j ** 2
            if temp > c:
                j -= 1
            elif temp < c:
                i += 1
            elif temp == c:
                return True
        return False
```