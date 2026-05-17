# 3022 Minimize OR of Remaining Elements Using Operations 给定操作次数内使剩余元素的或值最小

__Description:__

You are given a __0-indexed__ integer array `nums` and an integer `k`.

In one operation, you can pick any index `i` of `nums` such that `0 <= i < nums.length - 1` and replace `nums[i]` and `nums[i + 1]` with a single occurrence of `nums[i] & nums[i + 1]`, where `&` represents the bitwise `AND` operator.

Return _the __minimum__ possible value of the bitwise_ `OR` _of the remaining elements of_ `nums` _after applying __at most___ `k` _operations_.

__Example:__

Example 1:

```text
Input: nums = [3,5,3,2,7], k = 2
Output: 3
Explanation: Let's do the following operations:
```

1. Replace nums[0] and nums[1] with (nums[0] & nums[1]) so that nums becomes equal to [1,3,2,7].
2. Replace nums[2] and nums[3] with (nums[2] & nums[3]) so that nums becomes equal to [1,3,2].

The bitwise-or of the final array is 3.

It can be shown that 3 is the minimum possible value of the bitwise OR of the remaining elements of nums after applying at most k operations.

Example 2:

```text
Input: nums = [7,3,15,14,2,8], k = 4
Output: 2
Explanation: Let's do the following operations:
```

1. Replace nums[0] and nums[1] with (nums[0] & nums[1]) so that nums becomes equal to [3,15,14,2,8]. 
2. Replace nums[0] and nums[1] with (nums[0] & nums[1]) so that nums becomes equal to [3,14,2,8].
3. Replace nums[0] and nums[1] with (nums[0] & nums[1]) so that nums becomes equal to [2,2,8].
4. Replace nums[1] and nums[2] with (nums[1] & nums[2]) so that nums becomes equal to [2,0].

The bitwise-or of the final array is 2.

It can be shown that 2 is the minimum possible value of the bitwise OR of the remaining elements of nums after applying at most k operations.

Example 3:

```text
Input: nums = [10,7,10,3,9,14,9,4], k = 1
Output: 15
Explanation: Without applying any operations, the bitwise-or of nums is 15.
It can be shown that 15 is the minimum possible value of the bitwise OR of the remaining elements of nums after applying at most k operations.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] < 2 ^ 30`
- `0 <= k < nums.length`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `k` 。

一次操作中，你可以选择 `nums` 中满足 `0 <= i < nums.length - 1` 的一个下标 `i` ，并将 `nums[i]` 和 `nums[i + 1]` 替换为数字 `nums[i] & nums[i + 1]` ，其中 `&` 表示按位 `AND` 操作。

请你返回 __至多__ `k` 次操作以内，使 `nums` 中所有剩余元素按位 `OR` 结果的 __最小值__ 。

__示例:__

示例 1：

```text
输入：nums = [3,5,3,2,7], k = 2
输出：3
解释：执行以下操作：
```

1. 将 nums[0] 和 nums[1] 替换为 (nums[0] & nums[1]) ，得到 nums 为 [1,3,2,7] 。
2. 将 nums[2] 和 nums[3] 替换为 (nums[2] & nums[3]) ，得到 nums 为 [1,3,2] 。

最终数组的按位或值为 3 。

3 是 k 次操作以内，可以得到的剩余元素的最小按位或值。

示例 2：

```text
输入：nums = [7,3,15,14,2,8], k = 4
输出：2
解释：执行以下操作：
```

1. 将 nums[0] 和 nums[1] 替换为 (nums[0] & nums[1]) ，得到 nums 为 [3,15,14,2,8] 。
2. 将 nums[0] 和 nums[1] 替换为 (nums[0] & nums[1]) ，得到 nums 为 [3,14,2,8] 。
3. 将 nums[0] 和 nums[1] 替换为 (nums[0] & nums[1]) ，得到 nums 为 [2,2,8] 。
4. 将 nums[1] 和 nums[2] 替换为 (nums[1] & nums[2]) ，得到 nums 为 [2,0] 。

最终数组的按位或值为 2 。

2 是 k 次操作以内，可以得到的剩余元素的最小按位或值。

示例 3：

```text
输入：nums = [10,7,10,3,9,14,9,4], k = 1
输出：15
解释：不执行任何操作，nums 的按位或值为 15 。
15 是 k 次操作以内，可以得到的剩余元素的最小按位或值。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] < 2 ^ 30`
- `0 <= k < nums.length`

__思路:__

```text
位运算
对于 [3,5,3,2,7]
转化为二进制
0b011
0b101
0b011
0b010
0b111
从高位向低位遍历
如果要把最高位(从右往左第 2 位, 从 0 开始)变为 0
需要操作 2 次
即每次将 num 与 1 << 2(计入 mask) 进行与运算并统计为 1 的个数 cnt
如果 cnt 超过 k
那么无论如何该位都必须为 1
将这一位加入到结果中
并将 mask 取反, 因为后续该位不参与与运算
时间复杂度为 O(NlogM), 空间复杂度为 O(1), M 为 max(nums), logM 不超过 30
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOrAfterOperations(vector<int>& nums, int k) 
    {
        int result = 0;
        for (int i = 29, cur = -1, cnt = 0, mask = 1 << i; i > -1; i--, cur = -1, cnt = 0, mask |= 1 << max(i, 0)) 
        {
            for (int num : nums) 
            {
                if (cur &= num & mask) ++cnt;
                else cur = -1;
            }
            if (cnt > k) 
            {
                result |= 1 << i;
                mask ^= 1 << i;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOrAfterOperations(int[] nums, int k) {
        int result = 0;
        for (int i = 29, cur = -1, cnt = 0, mask = 1 << i; i > -1; i--, cur = -1, cnt = 0, mask |= 1 << Math.max(i, 0)) {
            for (int num : nums) {
                if ((cur &= num & mask) != 0) ++cnt;
                else cur = -1;
            }
            if (cnt > k) {
                result |= 1 << i;
                mask ^= 1 << i;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOrAfterOperations(self, nums: List[int], k: int) -> int:
        total, result, mask = max(nums).bit_length(), 0, 0
        for i in range(total - 1, -1, -1):
            cur, cnt = -1, 0
            mask |= 1 << i
            for num in nums:
                if (cur := cur & num & mask):
                    cnt += 1
                else:
                    cur = -1
            if cnt > k:
                result |= 1 << i
                mask ^= 1 << i
        return result
```
