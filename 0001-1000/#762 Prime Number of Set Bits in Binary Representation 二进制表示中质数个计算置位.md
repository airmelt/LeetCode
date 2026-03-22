# 762 Prime Number of Set Bits in Binary Representation 二进制表示中质数个计算置位

__Description__:
Given two integers L and R, find the count of numbers in the range [L, R] (inclusive) having a prime number of set bits in their binary representation.

(Recall that the number of set bits an integer has is the number of 1s present when written in binary. For example, 21 written in binary is 10101 which has 3 set bits. Also, 1 is not a prime.)

__Example:__

Example 1:

Input: L = 6, R = 10
Output: 4
Explanation:
6 -> 110 (2 set bits, 2 is prime)
7 -> 111 (3 set bits, 3 is prime)
9 -> 1001 (2 set bits , 2 is prime)
10->1010 (2 set bits , 2 is prime)

Example 2:

Input: L = 10, R = 15
Output: 5
Explanation:
10 -> 1010 (2 set bits, 2 is prime)
11 -> 1011 (3 set bits, 3 is prime)
12 -> 1100 (2 set bits, 2 is prime)
13 -> 1101 (3 set bits, 3 is prime)
14 -> 1110 (3 set bits, 3 is prime)
15 -> 1111 (4 set bits, 4 is not prime)

__Note:__

L, R will be integers L <= R in the range [1, 10^6].
R - L will be at most 10000.

__题目描述__:
给定两个整数 L 和 R ，找到闭区间 [L, R] 范围内，计算置位位数为质数的整数个数。

（注意，计算置位代表二进制表示中1的个数。例如 21 的二进制表示 10101 有 3 个计算置位。还有，1 不是质数。）

__示例 :__

示例 1:

输入: L = 6, R = 10
输出: 4
解释:
6 -> 110 (2 个计算置位，2 是质数)
7 -> 111 (3 个计算置位，3 是质数)
9 -> 1001 (2 个计算置位，2 是质数)
10-> 1010 (2 个计算置位，2 是质数)

示例 2:

输入: L = 10, R = 15
输出: 5
解释:
10 -> 1010 (2 个计算置位, 2 是质数)
11 -> 1011 (3 个计算置位, 3 是质数)
12 -> 1100 (2 个计算置位, 2 是质数)
13 -> 1101 (3 个计算置位, 3 是质数)
14 -> 1110 (3 个计算置位, 3 是质数)
15 -> 1111 (4 个计算置位, 4 不是质数)

__注意：__

L, R 是 L <= R 且在 [1, 10^6] 中的整数。
R - L 的最大值为 10000。

__思路__:

遍历[L, R], 利用 n & (n - 1)计算 1的位数
由于 L, R <= 10 ^ 6, 2 ^ 20 = 1048576, 所以质数最大不会超过 19
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int countPrimeSetBits(int L, int R) 
    {
        int prime[] = {0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1}, result = 0;
        for (int i = L; i < R + 1; i++) 
        {
            int count = 0, n = i;
            while (n) 
            {
                n &= n - 1;
                count++;
            }
            result += prime[count];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countPrimeSetBits(int L, int R) {
        int result = 0;
        // 665772二进制的各位刚好是质数所在位置
        for (int i = L; i < R + 1; i++) result += 665772 >> Integer.bitCount(i) & 1;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPrimeSetBits(self, L: int, R: int) -> int:
        return len([i for i in range(L, R + 1) if bin(i).count('1') in (2, 3, 5, 7, 11, 13, 17, 19)])
```
