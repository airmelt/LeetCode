# 2527 Find Xor-Beauty of Array 查询数组异或美丽值

__Description:__

You are given a __0-indexed__ integer array `nums`.

The __effective value__ of three indices `i`, `j`, and `k` is defined as `((nums[i] | nums[j]) & nums[k])`.

The __xor-beauty__ of the array is the XORing of __the effective values of all the possible triplets__ of indices `(i, j, k)` where `0 <= i, j, k < n`.

Return _the xor-beauty of_ `nums`.

Note that:

- `val1 | val2` is bitwise OR of `val1` and `val2`.
- `val1 & val2` is bitwise AND of `val1` and `val2`.

__Example:__

Example 1:

```text
Input: nums = [1,4]
Output: 5
Explanation: 
The triplets and their corresponding effective values are listed below:
```

- (0,0,0) with effective value ((1 | 1) & 1) = 1
- (0,0,1) with effective value ((1 | 1) & 4) = 0
- (0,1,0) with effective value ((1 | 4) & 1) = 1
- (0,1,1) with effective value ((1 | 4) & 4) = 4
- (1,0,0) with effective value ((4 | 1) & 1) = 1
- (1,0,1) with effective value ((4 | 1) & 4) = 4
- (1,1,0) with effective value ((4 | 4) & 1) = 0
- (1,1,1) with effective value ((4 | 4) & 4) = 4

Xor-beauty of array will be bitwise XOR of all beauties = 1  ^  0  ^  1  ^  4  ^  1  ^  4  ^  0  ^  4 = 5.

Example 2:

```text
Input:  nums = [15,45,20,2,34,35,5,44,32,30]
Output:  34
Explanation:  `The xor-beauty of the given array is 34.`
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。

三个下标 `i` ， `j` 和 `k` 的 __有效值__ 定义为 `((nums[i] | nums[j]) & nums[k])` 。

一个数组的 __异或美丽值__ 是数组中所有满足 `0 <= i, j, k < n` __的三元组__ `(i, j, k)` 的 __有效值__ 的异或结果。

请你返回 `nums` 的异或美丽值。

注意：

- `val1 | val2` 是 `val1` 和 `val2` 的按位或。
- `val1 & val2` 是 `val1` 和 `val2` 的按位与。

__示例:__

示例 1：

```text
输入：nums = [1,4]
输出：5
解释：
三元组和它们对应的有效值如下：
```

- (0,0,0) 有效值为 ((1 | 1) & 1) = 1
- (0,0,1) 有效值为 ((1 | 1) & 4) = 0
- (0,1,0) 有效值为 ((1 | 4) & 1) = 1
- (0,1,1) 有效值为 ((1 | 4) & 4) = 4
- (1,0,0) 有效值为 ((4 | 1) & 1) = 1
- (1,0,1) 有效值为 ((4 | 1) & 4) = 4
- (1,1,0) 有效值为 ((4 | 4) & 1) = 0
- (1,1,1) 有效值为 ((4 | 4) & 4) = 4

数组的异或美丽值为所有有效值的按位异或 1  ^  0  ^  1  ^  4  ^  1  ^  4  ^  0  ^  4 = 5 。

示例 2：

```text
_输入:_ nums = [15,45,20,2,34,35,5,44,32,30]
 _输出:_ 34
 ` _解释:_ 数组的异或美丽值为 34 。`
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
数学
比赛的时候可以直接猜结论
返回 nums 的异或值
以下是证明
对任意 i, j, k
if i == j == k, nums[i] | nums[i] & nums[i] = nums[i]
对于其他情况, 均有 (i, j, k), (j, i, k) 以及其他两种对称情况
nums[i] | nums[j] & nums[k] ^ (nums[j] | nums[i] & nums[j]) = 0
所以实际上只剩下 nums[i] 的异或, 即数组 nums 的异或值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int xorBeauty(vector<int>& nums) 
    {
        int result = 0;
        for (const auto& num : nums) result ^= num;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int xorBeauty(int[] nums) {
        int result = 0;
        for (int num : nums) result ^= num;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def xorBeauty(self, nums: List[int]) -> int:
        return reduce(xor, nums)
```
