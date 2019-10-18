__Description__:
Given a positive integer N, find and return the longest distance between two consecutive 1's in the binary representation of N.

If there aren't two consecutive 1's, return 0.

__Example:__
Example 1:

Input: 22
Output: 2
Explanation: 
22 in binary is 0b10110.
In the binary representation of 22, there are three ones, and two consecutive pairs of 1's.
The first consecutive pair of 1's have distance 2.
The second consecutive pair of 1's have distance 1.
The answer is the largest of these two distances, which is 2.

Example 2:

Input: 5
Output: 2
Explanation: 
5 in binary is 0b101.

Example 3:

Input: 6
Output: 1
Explanation: 
6 in binary is 0b110.

Example 4:

Input: 8
Output: 0
Explanation: 
8 in binary is 0b1000.
There aren't any consecutive pairs of 1's in the binary representation of 8, so we return 0.
 
__Note:__

1 <= N <= 10^9

__题目描述__:
给定一个正整数 N，找到并返回 N 的二进制表示中两个连续的 1 之间的最长距离。 

如果没有两个连续的 1，返回 0 。

__示例 :__
示例 1：

输入：22
输出：2
解释：
22 的二进制是 0b10110 。
在 22 的二进制表示中，有三个 1，组成两对连续的 1 。
第一对连续的 1 中，两个 1 之间的距离为 2 。
第二对连续的 1 中，两个 1 之间的距离为 1 。
答案取两个距离之中最大的，也就是 2 。

示例 2：

输入：5
输出：2
解释：
5 的二进制是 0b101 。

示例 3：

输入：6
输出：1
解释：
6 的二进制是 0b110 。

示例 4：

输入：8
输出：0
解释：
8 的二进制是 0b1000 。
在 8 的二进制表示中没有连续的 1，所以返回 0 。
 
__提示：__

1 <= N <= 10^9

__思路__:
1. 转化为字符串或者 bitset, 查找连续 1中间最长的距离即可, 距离可以存储在数组中或者用 last指针更新
时间复杂度O(lgn), 空间复杂度O(lgn)或O(1)
2. 用 last指针记录上一个 1出现的位置, 找到下一个 1就尝试更新最长的距离
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    int binaryGap(int N) {
        bitset<32>t(N);
        int result = 0, last = -1;
        for (int i = 0; i < 32; i++)
        {
            if (t[i]) 
            {
                if (last >= 0) result = max(result, i - last);
                last = i;
            }
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int binaryGap(int N) {
        int last = -1, result = 0;
        for (int i = 0; i < 32; ++i) {
            if (((N >> i) & 1) > 0) {
                if (last >= 0) result = Math.max(result, i - last);
                last = i;
            }
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def binaryGap(self, N: int) -> int:
        return max([len(i) + 1 for i in bin(N)[2:].split('1')[1:-1]] + [0]) 
```