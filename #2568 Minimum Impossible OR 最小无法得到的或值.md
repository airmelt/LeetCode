# 2568 Minimum Impossible OR 最小无法得到的或值

__Description:__

You are given a __0-indexed__ integer array `nums`.

We say that an integer x is __expressible__ from `nums` if there exist some integers `0 <= index1 < index2 < ... < index_k < nums.length` for which `nums[index1] | nums[index2] | ... | nums[index_k] = x`. In other words, an integer is expressible if it can be written as the bitwise OR of some subsequence of `nums`.

Return _the minimum __positive non-zero integer__ that is not_ _expressible from_ `nums`.

__Example:__

Example 1:

```text
Input: nums = [2,1]
Output: 4
Explanation: 1 and 2 are already present in the array. We know that 3 is expressible, since nums[0] | nums[1] = 2 | 1 = 3. Since 4 is not expressible, we return 4.
```

Example 2:

```text
Input: nums = [5,3,2]
Output: 1
Explanation: We can show that 1 is the smallest number that is not expressible.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。

如果存在一些整数满足 `0 <= index1 < index2 < ... < index_k < nums.length` ，得到 `nums[index1] | nums[index2] | ... | nums[index_k] = x` ，那么我们说 `x` 是 __可表达的__ 。换言之，如果一个整数能由 `nums` 的某个子序列的或运算得到，那么它就是可表达的。

请你返回 `nums` 不可表达的 __最小非零整数__ 。

__示例:__

示例 1：

```text
输入：nums = [2,1]
输出：4
解释：1 和 2 已经在数组中，因为 nums[0] | nums[1] = 2 | 1 = 3 ，所以 3 是可表达的。由于 4 是不可表达的，所以我们返回 4 。
```

示例 2：

```text
输入：nums = [5,3,2]
输出：1
解释：1 是最小不可表达的数字。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
位运算
如果 2 的 i 次方在 nums 中, 那么 [1, 2 ^ i - 1] 都是可表达的
所以从 0 开始检查 2 的 i 次方是否在 nums 中
如果不在, 那么 2 的 i 次方就是最小不可表达的数字
反之, 将所有 nums 中的 2 的 i 次方都标记为可表达的, 即取或运算的结果 mask
将 mask 取反, 再取 lowbit 即为答案
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minImpossibleOR(vector<int>& nums) 
    {
        int mask = 0;
        for (const auto& x : nums) if (!(x & (x - 1))) mask |= x;
        mask = ~mask;
        return mask & -mask;
    }
};
```

__Java__:

```Java
class Solution {
    public int minImpossibleOR(int[] nums) {
        int mask = 0;
        for (int x : nums) if ((x & (x - 1)) == 0) mask |= x;
        mask = ~mask;
        return mask & -mask;
    }
}
```

__Python__:

```Python
class Solution:
    def minImpossibleOR(self, nums: List[int]) -> int:
        return next(1 << i for i in count() if 1 << i not in set(nums))
```
