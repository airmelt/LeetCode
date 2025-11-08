# 2750 Ways to Split Array Into Good Subarrays 将数组划分成若干好子数组的方式

__Description:__

You are given a binary array `nums`.

A subarray of an array is __good__ if it contains __exactly__ __one__ element with the value `1`.

Return _an integer denoting the number of ways to split the array_ `nums` _into __good__ subarrays_. As the number may be too large, return it __modulo__ `10 ^ 9 + 7`.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [0,1,0,0,1]
Output: 3
Explanation: There are 3 ways to split nums into good subarrays:
```

- [0,1] [0,0,1]
- [0,1,0] [0,1]
- [0,1,0,0] [1]

Example 2:

```text
Input: nums = [0,1,0]
Output: 1
Explanation: There is 1 way to split nums into good subarrays:
```

- [0,1,0]

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 1`

__题目描述:__

给你一个二元数组 `nums` 。

如果数组中的某个子数组 __恰好__ 只存在 __一__ 个值为 `1` 的元素，则认为该子数组是一个 __好子数组__ 。

请你统计将数组 `nums` 划分成若干 __好子数组__ 的方法数，并以整数形式返回。由于数字可能很大，返回其对 `10 ^ 9 + 7` __取余__ 之后的结果。

子数组是数组中的一个连续 非空 元素序列。

__示例:__

示例 1：

```text
输入：nums = [0,1,0,0,1]
输出：3
解释：存在 3 种可以将 nums 划分成若干好子数组的方式：
```

- [0,1] [0,0,1]
- [0,1,0] [0,1]
- [0,1,0,0] [1]

示例 2：

```text
输入：nums = [0,1,0]
输出：1
解释：存在 1 种可以将 nums 划分成若干好子数组的方式：
```

- [0,1,0]

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 1`

__思路:__

```text
数学
数组中出现的第一个 1 必须和前面的 0 共同组成一组
后续出现的 0 可以放到任意一个组
根据乘法原理
计算出相邻的 1 中间的长度相乘即可
例外是全 0 的数组没有办法分割
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfGoodSubarraySplits(vector<int>& nums) 
    {
        long long result = 1LL, MOD = 1e9 + 7, pre = -1L;
        for (int i = 0, n = nums.size(); i < n; i++) 
        {
            if (nums[i] == 1 and pre > -1LL) result = result * (i - pre) % MOD;
            if (nums[i] == 1) pre = i;
        }
        return pre < 0LL ? 0 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfGoodSubarraySplits(int[] nums) {
        long result = 1L, MOD = 1_000_000_007L, pre = -1L;
        for (int i = 0, n = nums.length; i < n; i++) {
            if (nums[i] == 1 && pre > -1L) result = result * (i - pre) % MOD;
            if (nums[i] == 1) pre = i;
        }
        return pre < 0L ? 0 : (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfGoodSubarraySplits(self, nums: List[int]) -> int:
        return reduce(lambda x, y: x * y % (10 ** 9 + 7), [1] + [y - x for x, y in pairwise([i for i, v in enumerate(nums) if v])]) if 1 in nums else 0
```
