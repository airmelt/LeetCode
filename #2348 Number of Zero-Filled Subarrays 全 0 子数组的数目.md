# 2348 Number of Zero-Filled Subarrays 全 0 子数组的数目

__Description:__

Given an integer array `nums`, return _the number of __subarrays__ filled with_ `0`.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,3,0,0,2,0,0,4]
Output: 6
Explanation: 
There are 4 occurrences of [0] as a subarray.
There are 2 occurrences of [0,0] as a subarray.
There is no occurrence of a subarray with a size more than 2 filled with 0. Therefore, we return 6.
```

Example 2:

```text
Input: nums = [0,0,0,2,0,0]
Output: 9
Explanation:
There are 5 occurrences of [0] as a subarray.
There are 3 occurrences of [0,0] as a subarray.
There is 1 occurrence of [0,0,0] as a subarray.
There is no occurrence of a subarray with a size more than 3 filled with 0. Therefore, we return 9.
```

Example 3:

```text
Input: nums = [2,10,2019]
Output: 0
Explanation: There is no subarray filled with 0. Therefore, we return 0.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` ，返回全部为 `0` 的 __子数组__ 数目。

子数组 是一个数组中一段连续非空元素组成的序列。

__示例:__

示例 1：

```text
输入：nums = [1,3,0,0,2,0,0,4]
输出：6
解释：
子数组 [0] 出现了 4 次。
子数组 [0,0] 出现了 2 次。
不存在长度大于 2 的全 0 子数组，所以我们返回 6 。
```

示例 2：

```text
输入：nums = [0,0,0,2,0,0]
输出：9
解释：
子数组 [0] 出现了 5 次。
子数组 [0,0] 出现了 3 次。
子数组 [0,0,0] 出现了 1 次。
不存在长度大于 3 的全 0 子数组，所以我们返回 9 。
```

示例 3：

```text
输入：nums = [2,10,2019]
输出：0
解释：没有全 0 子数组，所以我们返回 0 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__思路:__

```text
计数
记录当前连续 0 的个数 c, 每次遇到 0 时, c += 1, 否则 c = 0
遍历数组, 依次将 c 加到结果中
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long zeroFilledSubarray(vector<int>& nums) 
    {
        long long result = 0LL, c = 0LL;
        for (const auto& num : nums) result += (c = !num ? c + 1LL : 0LL);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long zeroFilledSubarray(int[] nums) {
        long result = 0L, c = 0L;
        for (int num : nums) result += (c = num == 0 ? c + 1L : 0L);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def zeroFilledSubarray(self, nums: List[int]) -> int:
        return sum(comb(len([*g]) + 1, 2) for x, g in groupby(nums) if not x)
```
