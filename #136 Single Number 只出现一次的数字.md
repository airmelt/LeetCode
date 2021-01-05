# 136 Single Number 只出现一次的数字

__Description__:
Given a non-empty array of integers, every element appears twice except for one. Find that single one.

__Note__:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example:**

Example 1:
Input: [2,2,1]
Output: 1

Example 2:
Input: [4,1,2,1,2]
Output: 4

__题目描述__:
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

__说明__：
你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

**示例:**

示例 1:
输入: [2,2,1]
输出: 1

示例 2:
输入: [4,1,2,1,2]
输出: 4

__思路__:

利用位(异或)运算: n ^ n = 0, n ^ m ≠ 0,(n ≠ m)
可以取数组第一个数来进行运算, 避免额外空间
时间复杂度O(n), 空间复杂度O(1)

## 位运算

- & 与操作符 两个位都为1时，结果才为1
5 & 6 -> 0101 & 0110 = 0100 -> 4
- | 或操作符 两个位都为0时，结果才为0
5 | 6 -> 0101 | 0110 = 0111 -> 7
- ^ 异或操作符 两个位相同为0，相异为1
5 ^ 6 -> 0101 | 0110 = 0011 -> 3
- ~ 取反操作符 1 -> 0, 0 -> 1
~5 -> ~0101 = 1010 -> 10(取无符号整数)
- << 左移操作符 各二进位全部左移若干位，高位丢弃，低位补0, 相当于 * 2
5 << 1 -> 0101 << 1 = 1010 -> 10
- \>> 右移操作符 各二进位全部右移若干位，对无符号数，高位补0, 相当于 / 2
5 >> 1 -> 0101 >> 1 = 0010 -> 2

## 注意

1. 只有取反是单目运算符, 其余均是双目运算符
2. 位运算符优先级比较低, 与其他运算符号一起出现时, 应该使用括号
3. 位运算符也可以和 "="一起组合成赋值运算符: &=、|=、 ^=、<<=、>>=
4. 位运算符只能在int类型上使用

## 技巧

1. x & (x - 1) 消去 x最后一位1, 反复使用可以判断二进制 x中有多少位 '1'
2. x ^ x = 0
3. 不用中间变量交换两数: a ^= b; b ^= a; a ^= b;
4. 判断奇偶: x & 1 == 0 -> 奇数

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int singleNumber(vector<int>& nums) 
    {
        int result = 0;
        for (int num : nums) result ^= num;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int singleNumber(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            nums[0] ^= nums[i];
        }
        return nums[0];
    }
}
```

__Python__:

```Python
from functools import reduce
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        return reduce(lambda x, y: x ^ y, nums)
```
