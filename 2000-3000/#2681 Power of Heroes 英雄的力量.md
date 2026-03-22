# 2681 Power of Heroes 英雄的力量

__Description:__

You are given a __0-indexed__ integer array `nums` representing the strength of some heroes. The _power_ of a group of heroes is defined as follows:

- Let `i0`, `i1`, ... , `ik` be the indices of the heroes in a group. Then, the power of this group is `max(nums[i0], nums[i1], ... ,nums[ik]) ^ 2 * min(nums[i0], nums[i1], ... ,nums[ik])`.

Return _the sum of the __power__ of all __non-empty__ groups of heroes possible._ Since the sum could be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [2,1,4]
Output: 141
Explanation: 
1st group: [2] has power = 22 * 2 = 8.
2nd group: [1] has power = 12 * 1 = 1. 
3rd group: [4] has power = 42 * 4 = 64. 
4th group: [2,1] has power = 22 * 1 = 4. 
5th group: [2,4] has power = 42 * 2 = 32. 
6th group: [1,4] has power = 42 * 1 = 16. 
7th group: [2,1,4] has power = 42 * 1 = 16. 
The sum of powers of all groups is 8 + 1 + 64 + 4 + 32 + 16 + 16 = 141.
```

Example 2:

```text
Input: nums = [1,1,1]
Output: 7
Explanation: A total of 7 groups are possible, and the power of each group will be 1. Therefore, the sum of the powers of all groups is 7.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，它表示英雄的能力值。如果我们选出一部分英雄，这组英雄的 __力量__ 定义为:

- `i0` ， `i1` ，... `ik` 表示这组英雄在数组中的下标。那么这组英雄的力量为 `max(nums[i0],nums[i1] ... nums[ik]) ^ 2 * min(nums[i0],nums[i1] ... nums[ik])` 。

请你返回所有可能的 __非空__ 英雄组的 __力量__ 之和。由于答案可能非常大，请你将结果对 `10 ^ 9 + 7` __取余。__

__示例:__

示例 1：

```text
输入：nums = [2,1,4]
输出：141
解释：
第 1 组：[2] 的力量为 22 * 2 = 8 。
第 2 组：[1] 的力量为 12 * 1 = 1 。
第 3 组：[4] 的力量为 42 * 4 = 64 。
第 4 组：[2,1] 的力量为 22 * 1 = 4 。
第 5 组：[2,4] 的力量为 42 * 2 = 32 。
第 6 组：[1,4] 的力量为 42 * 1 = 16 。
第 7 组：[2,1,4] 的力量为 42 * 1 = 16 。
所有英雄组的力量之和为 8 + 1 + 64 + 4 + 32 + 16 + 16 = 141 。
```

示例 2：

```text
输入：nums = [1,1,1]
输出：7
解释：总共有 7 个英雄组，每一组的力量都是 1 。所以所有英雄组的力量之和为 7 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
贪心
由于需要求所有子数组力量之和
可以先对数组进行排序
接下来如果固定最大值
则对于最小值 i0 作为最小值的子数组有 2 ^ (n - 1) 个
对次小值 i1 作为最小值的子数组有 2 ^ (n - 2) 个
依次类推
所以每次求前缀和 pre
pre * num * num 即为力量和
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumOfPower(vector<int>& nums) 
    {
        long long result = 0LL, a = 0LL, MOD = 1e9 + 7LL;
        sort(nums.begin(), nums.end());
        for (const auto& num : nums) 
        {
            result = (result + (a + num) * ((long long)num * num % MOD)) % MOD;
            a = ((a << 1L) + num) % MOD;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumOfPower(int[] nums) {
        long result = 0L, a = 0L, MOD = 1_000_000_007L;
        Arrays.sort(nums);
        for (long num : nums) {
            result = (result + (a + num) * (num * num % MOD)) % MOD;
            a = ((a << 1L) + num) % MOD;
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfPower(self, nums: List[int]) -> int:
        nums.sort()
        a, result, MOD = 0, 0, 10 ** 9 + 7
        for v in nums:
            result = (result + (a + v) * v ** 2) % MOD
            a = ((a << 1) + v) % MOD
        return result
```
