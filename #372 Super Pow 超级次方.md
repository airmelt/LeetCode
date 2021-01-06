# 372 Super Pow 超级次方

__Description__:
Your task is to calculate ab mod 1337 where a is a positive integer and b is an extremely large positive integer given in the form of an array.

__Example:__

Example 1:

Input: a = 2, b = [3]
Output: 8

Example 2:

Input: a = 2, b = [1,0]
Output: 1024

Example 3:

Input: a = 1, b = [4,3,3,8,5,2]
Output: 1

Example 4:

Input: a = 2147483647, b = [2,0,0]
Output: 1198

__Constraints:__

1 <= a <= 2^31 - 1
1 <= b.length <= 2000
0 <= b[i] <= 9
b doesn't contain leading zeros.

__题目描述__:
你的任务是计算 ab 对 1337 取模，a 是一个正整数，b 是一个非常大的正整数且会以数组形式给出。

__示例 :__

示例 1：

输入：a = 2, b = [3]
输出：8

示例 2：

输入：a = 2, b = [1,0]
输出：1024

示例 3：

输入：a = 1, b = [4,3,3,8,5,2]
输出：1

示例 4：

输入：a = 2147483647, b = [2,0,0]
输出：1198

__提示：__

1 <= a <= 2^31 - 1
1 <= b.length <= 2000
0 <= b[i] <= 9
b 不含前导 0

__思路__:

快速幂
a ^ b = a \* a ^ (b / 2), b为奇数
a ^ b = a ^ (b / 2) \* a ^ (b / 2), b为偶数
递归
a ^ b = a ^ b[-1] \* (a ^ b[:-1]) ^ 10, b为数组
比如 2 ^ [1, 1] -> 2 ^ 1 \* (2 ^ [1]) ^ 10 -> 2 \* 1024 = 2048
取余
ab % base = (a % base) * (b % base)
所以每次只要有乘法, 就取一次余
时间复杂度O(nlgm), 空间复杂度O(n), 其中 n为数组 b的长度, m为数组 b中的最大值, 实际上 m最大为 9, 时间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    int base = 1337;
    
    int i = 0;
    
    int my_pow(int a, int b)
    {
        return b ? ((b & 1) ? (((a % base) * my_pow((a % base), b - 1)) % base) : (my_pow((a % base), b / 2) * my_pow((a % base), b / 2) % base)) : 1;
    }
public:
    int superPow(int a, vector<int>& b) 
    {
        return i == b.size() ? 1 : ((my_pow(a, b[b.size() - 1 - i++]) * my_pow(superPow(a, b), 10)) % base);
    }
};
```

__Java__:

```Java
class Solution {
    private final int BASE = 1337;
    
    private int i = 0;
    
    private int helper(int a, int b) {
        return b != 0 ? (((b & 1) != 0) ? (((a % BASE) * helper((a % BASE), b - 1)) % BASE) : (helper((a % BASE), b / 2) * helper((a % BASE), b / 2) % BASE)) : 1; 
    }
    
    public int superPow(int a, int[] b) {
        return b.length == i ? 1 : ((helper(a, b[b.length - 1 - i++]) * helper(superPow(a, b), 10)) % BASE);
    }
}
```

__Python__:

```Python
class Solution:
    def superPow(self, a: int, b: List[int]) -> int:
        return pow(a, int(''.join([str(i) for i in b])), 1337)
```
