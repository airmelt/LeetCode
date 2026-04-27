# 1375 Number of Times Binary String Is Prefix-Aligned 二进制字符串前缀一致的次数

__Description:__

You have a 1-indexed binary string of length n where all the bits are 0 initially. We will flip all the bits of this binary string (i.e., change them from 0 to 1) one by one. You are given a 1-indexed integer array flips where flips[i] indicates that the bit at index i will be flipped in the ith step.

A binary string is prefix-aligned if, after the ith step, all the bits in the inclusive range [1, i] are ones and all the other bits are zeros.

Return the number of times the binary string is prefix-aligned during the flipping process.

__Example:__

Example 1:

Input: flips = [3,2,4,1,5]
Output: 2
Explanation: The binary string is initially "00000".
After applying step 1: The string becomes "00100", which is not prefix-aligned.
After applying step 2: The string becomes "01100", which is not prefix-aligned.
After applying step 3: The string becomes "01110", which is not prefix-aligned.
After applying step 4: The string becomes "11110", which is prefix-aligned.
After applying step 5: The string becomes "11111", which is prefix-aligned.
We can see that the string was prefix-aligned 2 times, so we return 2.

Example 2:

Input: flips = [4,1,2,3]
Output: 1
Explanation: The binary string is initially "0000".
After applying step 1: The string becomes "0001", which is not prefix-aligned.
After applying step 2: The string becomes "1001", which is not prefix-aligned.
After applying step 3: The string becomes "1101", which is not prefix-aligned.
After applying step 4: The string becomes "1111", which is prefix-aligned.
We can see that the string was prefix-aligned 1 time, so we return 1.

__Constraints:__

n == flips.length
1 <= n <= 5 \* 10^4
flips is a permutation of the integers in the range [1, n].

__题目描述：__

给你一个长度为 n 、下标从 1 开始的二进制字符串，所有位最开始都是 0 。我们会按步翻转该二进制字符串的所有位（即，将 0 变为 1）。

给你一个下标从 1 开始的整数数组 flips ，其中 flips[i] 表示对应下标 i 的位将会在第 i 步翻转。

二进制字符串 前缀一致 需满足：在第 i 步之后，在 闭 区间 [1, i] 内的所有位都是 1 ，而其他位都是 0 。

返回二进制字符串在翻转过程中 前缀一致 的次数。

__示例：__

示例 1：

输入：flips = [3,2,4,1,5]
输出：2
解释：二进制字符串最开始是 "00000" 。
执行第 1 步：字符串变为 "00100" ，不属于前缀一致的情况。
执行第 2 步：字符串变为 "01100" ，不属于前缀一致的情况。
执行第 3 步：字符串变为 "01110" ，不属于前缀一致的情况。
执行第 4 步：字符串变为 "11110" ，属于前缀一致的情况。
执行第 5 步：字符串变为 "11111" ，属于前缀一致的情况。
在翻转过程中，前缀一致的次数为 2 ，所以返回 2 。

示例 2：

输入：flips = [4,1,2,3]
输出：1
解释：二进制字符串最开始是 "0000" 。
执行第 1 步：字符串变为 "0001" ，不属于前缀一致的情况。
执行第 2 步：字符串变为 "1001" ，不属于前缀一致的情况。
执行第 3 步：字符串变为 "1101" ，不属于前缀一致的情况。
执行第 4 步：字符串变为 "1111" ，属于前缀一致的情况。
在翻转过程中，前缀一致的次数为 1 ，所以返回 1 。

__提示：__

n == flips.length
1 <= n <= 5 * 104
flips 是范围 [1, n] 中所有整数构成的一个排列

__思路：__

DFS
注意到 BST 的左子树和右子树都为 BST
且根结点大于左子树的最大值和小于右子树的最小值
用一个全局变量记录 BST 的最大和
后序遍历搜索所有结点并记录下当前和, 最大值和最小值
左子树初始化为 {0, root -> val, -INF}
右子树初始化为 {0, INF, root -> val}
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int numTimesAllBlue(vector<int>& flips)
    {
        int n = flips.size(), result = 0, reach = 0;
        for (int i = 0 ; i < n; i++) result += (i + 1 == (reach = max(reach, flips[i])));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numTimesAllBlue(int[] flips) {
        int n = flips.length, result = 0, reach = 0;
        for (int i = 0 ; i < n; i++) if (i + 1 == (reach = Math.max(reach, flips[i]))) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numTimesAllBlue(self, flips: List[int]) -> int:
        result, n, reach = 0, len(flips), 0
        for i in range(n):
            result += (i + 1 == (reach := max(reach, flips[i])))
        return result
```
