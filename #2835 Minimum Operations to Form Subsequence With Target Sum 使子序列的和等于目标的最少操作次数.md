# 2835 Minimum Operations to Form Subsequence With Target Sum 使子序列的和等于目标的最少操作次数

__Description:__

You are given a __0-indexed__ array `nums` consisting of __non-negative__ powers of `2`, and an integer `target`.

In one operation, you must apply the following changes to the array:

- Choose any element of the array `nums[i]` such that `nums[i] > 1`.
- Remove `nums[i]` from the array.
- Add __two__ occurrences of `nums[i] / 2` to the __end__ of `nums`.

Return the ___minimum number of operations__ you need to perform so that_ `nums` _contains a __subsequence__ whose elements sum to_ `target`. If it is impossible to obtain such a subsequence, return `-1`.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

```text
Input: nums = [1,2,8], target = 7
Output: 1
Explanation: In the first operation, we choose element nums[2]. The array becomes equal to nums = [1,2,4,4].
At this stage, nums contains the subsequence [1,2,4] which sums up to 7.
It can be shown that there is no shorter sequence of operations that results in a subsequence that sums up to 7.
```

Example 2:

```text
Input: nums = [1,32,1,2], target = 12
Output: 2
Explanation: In the first operation, we choose element nums[1]. The array becomes equal to nums = [1,1,2,16,16].
In the second operation, we choose element nums[3]. The array becomes equal to nums = [1,1,2,16,8,8]
At this stage, nums contains the subsequence [1,1,2,8] which sums up to 12.
It can be shown that there is no shorter sequence of operations that results in a subsequence that sums up to 12.
```

Example 3:

```text
Input: nums = [1,32,1], target = 35
Output: -1
Explanation: It can be shown that no sequence of operations results in a subsequence that sums up to 35.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 2 ^ 30`
- `nums` consists only of non-negative powers of two.
- `1 <= target < 2 ^ 31`

__题目描述:__

给你一个下标从 __0__ 开始的数组 `nums` ，它包含 __非负__ 整数，且全部为 `2` 的幂，同时给你一个整数 `target` 。

一次操作中，你必须对数组做以下修改：

- 选择数组中一个元素 `nums[i]` ，满足 `nums[i] > 1` 。
- 将 `nums[i]` 从数组中删除。
- 在 `nums` 的 __末尾__ 添加 __两个__ 数，值都为 `nums[i] / 2` 。

你的目标是让 `nums` 的一个 __子序列__ 的元素和等于 `target` ，请你返回达成这一目标的 __最少操作次数__ 。如果无法得到这样的子序列，请你返回 `-1` 。

数组中一个 子序列 是通过删除原数组中一些元素，并且不改变剩余元素顺序得到的剩余数组。

__示例:__

示例 1：

```text
输入：nums = [1,2,8], target = 7
输出：1
解释：第一次操作中，我们选择元素 nums[2] 。数组变为 nums = [1,2,4,4] 。
这时候，nums 包含子序列 [1,2,4] ，和为 7 。
无法通过更少的操作得到和为 7 的子序列。
```

示例 2：

```text
输入：nums = [1,32,1,2], target = 12
输出：2
解释：第一次操作中，我们选择元素 nums[1] 。数组变为 nums = [1,1,2,16,16] 。
第二次操作中，我们选择元素 nums[3] 。数组变为 nums = [1,1,2,16,8,8] 。
这时候，nums 包含子序列 [1,1,2,8] ，和为 12 。
无法通过更少的操作得到和为 12 的子序列。
```

示例 3：

```text
输入：nums = [1,32,1], target = 35
输出：-1
解释：无法得到和为 35 的子序列。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 2 ^ 30`
- `nums` 只包含非负整数，且均为 2 的幂。
- `1 <= target < 2 ^ 31`

__思路:__

```text
贪心
如果将所有的数字都改为 1 那么最多能组成 sum(nums)
如果 sum(nums) < target, 那么一定不能组成 target, 返回 -1
否则从最低位开始累加上对应位的值
如果比 target 当前位要小, 可以用 target & mask, 其中 mask = 1 << (i + 1) - 1 得到, 那么需要用高位的值拆一个 2 ^ i 出来
找到第一个大于 2 ^ i 的值 j, 在得到 2 ^ i 的同时也能得到 2 ^ (i + 1), 2 ^ (i + 2), ..., 2 ^ (j - 1)
下一次可以直接从 j 开始继续查找
时间复杂度为 O(NlogM), 空间复杂度为 O(logM), N 为 nums 的长度, M 为 target 的值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums, int target) 
    {
        if (accumulate(nums.begin(), nums.end(), 0LL) < target) return -1;
        unordered_map<int, int> m;
        for (const auto& num : nums) ++m[num];
        int result = 0, i = 0;
        long long s = 0LL, mask = 0LL;
        while ((1LL << i) <= target) 
        {
            s += (long long)m[1 << i] << i;
            if (s < (target & (mask = (1LL << (++i)) - 1LL))) 
            {
                ++result;
                while (!m[1 << i]) 
                {
                    ++result;
                    ++i;
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(List<Integer> nums, int target) {
        if (nums.stream().mapToLong(Integer::longValue).sum() < target) return -1;
        var map = new HashMap<Integer, Integer>();
        for (int num : nums) map.merge(num, 1, Integer::sum);
        int result = 0, i = 0;
        long s = 0L, mask = 0L;
        while ((1L << i) <= target) {
            s += (long)map.getOrDefault(1 << i, 0) << i;
            if (s < (target & (mask = (1L << (++i)) - 1L))) {
                ++result;
                while (map.getOrDefault(1 << i, 0) == 0) {
                    ++result;
                    ++i;
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int], target: int) -> int:
        if sum(nums) < target:
            return -1
        c, result, i, s = Counter(nums), 0, 0, 0
        while 1 << i <= target:
            s += c[1 << i] << i
            i += 1
            if s < target & (mask := (1 << i) - 1):
                result += 1
                while not c[1 << i]:
                    result += 1
                    i += 1
        return result
```
