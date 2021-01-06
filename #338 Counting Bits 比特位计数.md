# 338 Counting Bits 比特位计数

__Description__:
Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

__Example:__

Example 1:

Input: 2
Output: [0,1,1]

Example 2:

Input: 5
Output: [0,1,1,2,1,2]

__Follow up:__

It is very easy to come up with a solution with run time O(n*sizeof(integer)). But can you do it in linear time O(n) /possibly in a single pass?
Space complexity should be O(n).
Can you do it like a boss? Do it without using any builtin function like __builtin_popcount in c++ or in any other language.

__题目描述__:
给定一个非负整数 num。对于 0 ≤ i ≤ num 范围中的每个数字 i ，计算其二进制数中的 1 的数目并将它们作为数组返回。

__示例 :__

示例 1:

输入: 2
输出: [0,1,1]

示例 2:

输入: 5
输出: [0,1,1,2,1,2]

__进阶:__

给出时间复杂度为O(n*sizeof(integer))的解答非常容易。但你可以在线性时间O(n)内用一趟扫描做到吗？
要求算法的空间复杂度为O(n)。
你能进一步完善解法吗？要求在C++或任何其他语言中不使用任何内置函数（如 C++ 中的 __builtin_popcount）来执行此操作。

__思路__:

动态规划
dp[i]表示这一位对应的整数有 n个 1
对于每一个 i, i >> 1是已经计算过的, 如果 i是偶数, 则 i和 i >> 1的 1的个数相同, 如果 i是奇数, 则 i比 i >> 1的 1个个数多 1, 就是加上 i % 2的结果(即 i & 1)
如 9(1001) -> 4(100), 2 -> 1
动态转移方程: dp[i] = dp[i >> 1] + (i & 1)
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> countBits(int num) 
    {
        vector<int> result(num + 1);
        for (int i = 0; i < num + 1; i++) result[i] = result[i >> 1] + (i & 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] countBits(int num) {
        int result[] = new int[num + 1];
        for (int i = 0; i < num + 1; i++) result[i] = result[i >> 1] + (i & 1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countBits(self, num: int) -> List[int]:
        result = [0] * (num + 1)
        for i in range(num + 1):
            result[i] = result[i >> 1] + (i & 1)
        return result
```
