# 552 Student Attendance Record II 学生出勤记录 II

__Description__:
An attendance record for a student can be represented as a string where each character signifies whether the student was absent, late, or present on that day. The record only contains the following three characters:

'A': Absent.
'L': Late.
'P': Present.
Any student is eligible for an attendance award if they meet both of the following criteria:

The student was absent ('A') for strictly fewer than 2 days total.
The student was never late ('L') for 3 or more consecutive days.
Given an integer n, return the number of possible attendance records of length n that make a student eligible for an attendance award. The answer may be very large, so return it modulo 10^9 + 7.

__Example:__

Example 1:

Input: n = 2
Output: 8
Explanation: There are 8 records with length 2 that are eligible for an award:
"PP", "AP", "PA", "LP", "PL", "AL", "LA", "LL"
Only "AA" is not eligible because there are 2 absences (there need to be fewer than 2).

Example 2:

Input: n = 1
Output: 3

Example 3:

Input: n = 10101
Output: 183236316

__Constraints:__

1 <= n <= 10^5

__题目描述__:
给定一个正整数 n，返回长度为 n 的所有可被视为可奖励的出勤记录的数量。 答案可能非常大，你只需返回结果mod 10^9 + 7的值。

学生出勤记录是只包含以下三个字符的字符串：

'A' : Absent，缺勤
'L' : Late，迟到
'P' : Present，到场
如果记录不包含多于一个'A'（缺勤）或超过两个连续的'L'（迟到），则该记录被视为可奖励的。

__示例 :__

示例 1:

输入: n = 2
输出: 8
解释：
有8个长度为2的记录将被视为可奖励：
"PP" , "AP", "PA", "LP", "PL", "AL", "LA", "LL"
只有"AA"不会被视为可奖励，因为缺勤次数超过一次。

__注意:__
n 的值不会超过100000。

__思路__:

动态规划
可以画出状态转移图, 注意到 A 超过 1 次就不能奖励, 主要记录 A 的个数
记 P 为不包含 A 的状态, 且这次的记录为 P, AP 为包含一个 A 的状态, 且这次的记录为 P, L 为不包含 A 的状态, 且这次的记录为 L, AL 为包含一个 A 的状态, 且这次的记录为 L, LL 为不包含 A 的状态, 且最近两次的记录为 L, ALL 为包含 A 的状态, 且最近两次的记录为 L, A 表示这次的记录为 A
先去掉 n == 0 的特殊情况
对于 n == 1
P, L, A 记录都可以奖励, 这时三种情况都记为 1
即初始化 P = 1, L = 1, A = 1, AP = 0, AL, = 0, LL = 0, ALL = 0
每次遍历时
P = (P + L + LL),
L = P,
A = (P + L + LL),
AP = (AP + AL + ALL + A),
AL = (A + AP)
ALL = AL
LL = L
时间复杂度 O(n), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    const long M = 1000000007;
public:
    int checkRecord(int n) 
    {
        long a0l0 = 1, a0l1 = 0, a0l2 = 0, a1l0 = 0, a1l1 = 0, a1l2 = 0;
        for (int i = 0; i <= n; i++) 
        {
            long a0l0_ = (a0l0 + a0l1 + a0l2) % M;
            a0l2 = a0l1;
            a0l1 = a0l0;
            a0l0 = a0l0_;
            long a1l0_ = (a0l0 + a1l0 + a1l1 + a1l2) % M;
            a1l2 = a1l1;
            a1l1 = a1l0;
            a1l0 = a1l0_;
        }
        return (int) a1l0;
    }
};
```

__Java__:

```Java
class Solution {
    private static final int M = 1000000007; 
    public int checkRecord(int n) {
        long P = 1, L = 1, A = 1, AL = 0, ALL = 0, AP = 0, LL = 0;
        for (int i = 2; i <= n; i++) {
            long tempP = (P + L + LL) % M;
            long tempA = (AP + AL + ALL + A) % M;
            ALL = AL;
            AL = (A + AP) % M;
            LL = L;
            L = P;
            P = tempP;
            A = tempP;
            AP = tempA;
        }
        return (int)((P + AP + L + LL + AL + ALL + A) % M);
    }
}
```

__Python__:

```Python
class Solution:
    def checkRecord(self, n: int) -> int:
        M, P, AP, L, LL, AL, ALL, A = 10 ** 9 + 7, 1, 0, 1, 0, 0, 0, 1
        for i in range(2, n + 1):
            P, AP, L, LL, AL, ALL, A = (P + L + LL) % M, (AP + AL + ALL + A) % M, P, L, (AP + A) % M, AL, (P + L + LL) % M
        return (P + AP + L + LL + AL + ALL + A) % M
```
