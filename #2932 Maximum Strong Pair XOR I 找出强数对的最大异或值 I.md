# 2932 Maximum Strong Pair XOR I 找出强数对的最大异或值 I

__Description:__

You are given a __0-indexed__ integer array `nums`. A pair of integers `x` and `y` is called a __strong__ pair if it satisfies the condition:

- `|x - y| <= min(x, y)`

You need to select two integers from `nums` such that they form a strong pair and their bitwise `XOR` is the __maximum__ among all strong pairs in the array.

Return _the __maximum___ `XOR` _value out of all possible strong pairs in the array_ `nums`.

Note that you can pick the same integer twice to form a pair.

__Example:__

Example 1:

```text
Input:  nums = [1,2,3,4,5]
Output:  7
Explanation:  There are 11 strong pairs in the array nums: (1, 1), (1, 2), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (3, 5), (4, 4), (4, 5) and (5, 5).
The maximum XOR possible from these pairs is 3 XOR 4 = 7.
```

Example 2:

```text
Input:  nums = [10,100]
Output:  0
Explanation:  There are 2 strong pairs in the array nums: (10, 10) and (100, 100).
The maximum XOR possible from these pairs is 10 XOR 10 = 0 since the pair (100, 100) also gives 100 XOR 100 = 0.
```

Example 3:

```text
Input:  nums = [5,6,25,30]
Output:  7
Explanation:  There are 6 strong pairs in the array nums: (5, 5), (5, 6), (6, 6), (25, 25), (25, 30) and (30, 30).
The maximum XOR possible from these pairs is 25 XOR 30 = 7 since the only other non-zero XOR value is 5 XOR 6 = 3.
```

__Constraints:__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。如果一对整数 `x` 和 `y` 满足以下条件，则称其为 __强数对__ :

- `|x - y| <= min(x, y)`

你需要从 `nums` 中选出两个整数，且满足:这两个整数可以形成一个强数对，并且它们的按位异或（ `XOR`）值是在该数组所有强数对中的 __最大值__ 。

返回数组 `nums` 所有可能的强数对中的 __最大__ 异或值。

注意，你可以选择同一个整数两次来形成一个强数对。

__示例:__

示例 1：

```text
输入: nums = [1,2,3,4,5]
输出: 7
解释: 数组 nums 中有 11 个强数对:(1, 1), (1, 2), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (3, 5), (4, 4), (4, 5) 和 (5, 5) 。
这些强数对中的最大异或值是 3 XOR 4 = 7 。
```

示例 2：

```text
输入: nums = [10,100]
输出: 0
解释: 数组 nums 中有 2 个强数对:(10, 10) 和 (100, 100) 。
这些强数对中的最大异或值是 10 XOR 10 = 0 ，数对 (100, 100) 的异或值也是 100 XOR 100 = 0 。
```

示例 3：

```text
输入: nums = [5,6,25,30]
输出: 7
解释: 数组 nums 中有 6 个强数对:(5, 5), (5, 6), (6, 6), (25, 25), (25, 30) 和 (30, 30) 。
这些强数对中的最大异或值是 25 XOR 30 = 7 ；另一个异或值非零的数对是 (5, 6) ，其异或值是 5 XOR 6 = 3 。
```

__提示：__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 100`

__思路:__

```text
暴力法
因为 n 的范围仅有 50
可以直接使用暴力法
遍历每一个数对
满足题意的就更新异或值
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumStrongPairXor(vector<int>& nums) 
    {
        int result = 0;
        for (const auto& x : nums) for (const auto& y : nums) if (abs(x - y) <= min(x, y)) result = max(result, x ^ y);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumStrongPairXor(int[] nums) {
        int result = 0;
        for (int x : nums) for (int y : nums) if (Math.abs(x - y) <= Math.min(x, y)) result = Math.max(result, x ^ y);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumStrongPairXor(self, nums: List[int]) -> int:
        return max(x ^ y for x in nums for y in nums if abs(x - y) <= min(x, y))
```
