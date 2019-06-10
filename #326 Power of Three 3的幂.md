__Description__:
Given an integer, write a function to determine if it is a power of three.

**Example :**
Example 1:
Input: 27
Output: true

Example 2:
Input: 0
Output: false

Example 3:
Input: 9
Output: true

Example 4:
Input: 45
Output: false


__Follow up:__
Could you do it without using any loop / recursion?

__题目描述__:
给定一个整数，写一个函数来判断它是否是 3 的幂次方。

**示例 :**
示例 1:
输入: 27
输出: true

示例 2:
输入: 0
输出: false

示例 3:
输入: 9
输出: true

示例 4:
输入: 45
输出: false

__进阶：__
你能不使用循环或者递归来完成本题吗？

__思路__:
1. 循环判断 n % 3 == 0, 执行 n /= 3直到 n == 1
2. 递归, 方法同 1
3.注意到 3是质数, 所以 3的幂只有 3的 n次方的因数
如 9只有 1、3、9三个因子
int范围内最大的 3的幂为 1162261467(3 ^ 19)
3的幂一定是该数的因子, 即 1162261467 % n = 0
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    bool isPowerOfThree(int n) {
        return n > 0 && (1162261467 % n) == 0;
    }
};
```

__Java__:
```
class Solution {
    public boolean isPowerOfThree(int n) {
        return n > 0 && (1162261467 % n) == 0;
    }
}
```

__Python__:
```
class Solution:
    def isPowerOfThree(self, n: int) -> bool:
        return n > 0 and (1162261467 % n) == 0
```
