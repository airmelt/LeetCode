__Description__:
Given a positive integer, check whether it has alternating bits: namely, if two adjacent bits will always have different values.

__Example:__
Example 1:

Input: 5
Output: True
Explanation:
The binary representation of 5 is: 101

Example 2:

Input: 7
Output: False
Explanation:
The binary representation of 7 is: 111.

Example 3:

Input: 11
Output: False
Explanation:
The binary representation of 11 is: 1011.

Example 4:

Input: 10
Output: True
Explanation:
The binary representation of 10 is: 1010.

__题目描述__:
给定一个正整数，检查他是否为交替位二进制数：换句话说，就是他的二进制数相邻的两个位数永不相等。

__示例 :__
示例 1:

输入: 5
输出: True
解释:
5的二进制数是: 101

示例 2:

输入: 7
输出: False
解释:
7的二进制数是: 111

示例 3:

输入: 11
输出: False
解释:
11的二进制数是: 1011

 示例 4:

输入: 10
输出: True
解释:
10的二进制数是: 1010

__思路__:
1. 可以转换成字符串, 查找 11或者 00是否在字符串中, 或者检查相邻两位是否相同
2. 由于是相邻两位不同, 可以比较 n // 2和 n % 2的最后一位, 如果相同, 则说明相邻位相同, 返回 false
时间复杂度O(1), 空间复杂度O(1), 检查的数的范围为 int, 最多需要检查 32位数字

__代码__:
__C++__:
```
class Solution {
public:
    bool hasAlternatingBits(int n) {
        return ((n ^ (n >> 1)) & ((long)(n ^ (n >> 1)) + 1)) == 0;
    }
};
```

__Java__:
```
class Solution {
    public boolean hasAlternatingBits(int n) {
        return ((n ^ (n >> 1)) & ((long)(n ^ (n >> 1)) + 1)) == 0;
    }
}
```

__Python__:
```
class Solution:
    def hasAlternatingBits(self, n: int) -> bool:
        return '11' not in bin(n) and '00' not in bin(n)
```