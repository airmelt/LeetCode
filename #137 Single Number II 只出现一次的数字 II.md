__Description__:
Given a non-empty array of integers, every element appears three times except for one, which appears exactly once. Find that single one.

__Note:__

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

__Example:__
Example 1:

Input: [2,2,3,2]
Output: 3

Example 2:

Input: [0,1,0,1,0,1,99]
Output: 99

__题目描述__:
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现了三次。找出那个只出现了一次的元素。

__说明：__

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

__示例 :__
示例 1:

输入: [2,2,3,2]
输出: 3

示例 2:

输入: [0,1,0,1,0,1,99]
输出: 99

__思路__:
1.  用掩码的方式, 记录每一位出现的次数, 出现次数不为 3的倍数的加入结果
时间复杂度O(n), 空间复杂度O(1)
2. 参考[LeetCode #136 Single Number 只出现一次的数字](https://www.jianshu.com/p/d8050ac9d91d)
这里需要去掉出现 3次的数字
1位异或只能去掉 2次
考虑使用 2位异或
如果 x -> ? -> ? -> 0, 就能满足题意
可以设计为 00 -> 01 -> 10 -> 00, 即每一次取出一位将其翻转
需要有两个变量完成
```
b = (b ^ x) & ~a;
a = (a ^ x) & ~b;
```
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int singleNumber(vector<int>& nums) 
    {
        int result = 0;
        for (int i = 0; i < 32; i++)
        {
            int count = 0, mask = (1 << i);
            for (auto num : nums) if ((num & mask) != 0) ++count;
            if (count % 3 != 0) result |= mask;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int singleNumber(int[] nums) {
        int a = 0, b = 0;
        for (int num : nums) {
            a = ~b & (a ^ num);
            b = ~a & (b ^ num);
        }
        return a;
    }
}
```

__Python__:
```Python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        return Counter(nums).most_common()[-1][0]
```