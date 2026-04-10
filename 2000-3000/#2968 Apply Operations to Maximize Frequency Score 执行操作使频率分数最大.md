# 2968 Apply Operations to Maximize Frequency Score 执行操作使频率分数最大

__Description:__

You are given a __0-indexed__ integer array `nums` and an integer `k`.

You can perform the following operation on the array __at most__ `k` times:

- Choose any index `i` from the array and __increase__ or __decrease__ `nums[i]` by `1`.

The score of the final array is the frequency of the most frequent element in the array.

Return the maximum score you can achieve.

The frequency of an element is the number of occurences of that element in the array.

__Example:__

Example 1:

```text
Input: nums = [1,2,6,4], k = 3
Output: 3
Explanation: We can do the following operations on the array:
```

- Choose i = 0, and increase the value of nums[0] by 1. The resulting array is [2,2,6,4].
- Choose i = 3, and decrease the value of nums[3] by 1. The resulting array is [2,2,6,3].
- Choose i = 3, and decrease the value of nums[3] by 1. The resulting array is [2,2,6,2].

The element 2 is the most frequent in the final array so our score is 3.

It can be shown that we cannot achieve a better score.

Example 2:

```text
Input: nums = [1,4,4,2,4], k = 0
Output: 3
Explanation: We cannot apply any operations so our score will be the frequency of the most frequent element in the original array, which is 3.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `0 <= k <= 10 ^ 14`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `k` 。

你可以对数组执行 __至多__ `k` 次操作:

- 从数组中选择一个下标 `i` ，将 `nums[i]` __增加__ 或者 __减少__ `1` 。

最终数组的频率分数定义为数组中众数的 频率 。

请你返回你可以得到的 最大 频率分数。

众数指的是数组中出现次数最多的数。一个元素的频率指的是数组中这个元素的出现次数。

__示例:__

示例 1：

```text
输入：nums = [1,2,6,4], k = 3
输出：3
解释：我们可以对数组执行以下操作：
```

- 选择 i = 0 ，将 nums[0] 增加 1 。得到数组 [2,2,6,4] 。
- 选择 i = 3 ，将 nums[3] 减少 1 ，得到数组 [2,2,6,3] 。
- 选择 i = 3 ，将 nums[3] 减少 1 ，得到数组 [2,2,6,2] 。

元素 2 是最终数组中的众数，出现了 3 次，所以频率分数为 3 。

3 是所有可行方案里的最大频率分数。

示例 2：

```text
输入：nums = [1,4,4,2,4], k = 0
输出：3
解释：我们无法执行任何操作，所以得到的频率分数是原数组中众数的频率 3 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `0 <= k <= 10 ^ 14`

__思路:__

```text
贪心 ➕ 滑动窗口
先对数组进行排序
这样选择相邻的数字能够选的更多
因为需要增加或者减少的次数会变少
最优解是将所有数字变成滑动窗口的中位数
能取到最少的操作次数
记录滑动窗口的最长的长度就是答案
对于滑动窗口左边界为 left 右边界 right
设中位数 mid = left + right >> 1
1. 前缀和
所有的数字最终都要变成中位数 nums[mid]
那么对于 mid 左边的都需要减去
即 nums[mid] - nums[mid - 1] + nums[mid] - nums[mid - 2] + ... + nums[mid] - nums[left]
即 nums[mid] * (mid - left) - (pre[mid] - pre[left])
同理可得 mid 右边的和为 pre[right + 1] - pre[mid + 1] - nums[mid] * (right - mid)
若滑动窗口内的操作数大于 k, 则滑动窗口的左边界需要往右移动
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
2. 贡献法
如果滑动窗口的右边界往右边移动
那么一定会改变中位数的位置
从而贡献量会减少
同时由于新的元素进入窗口
这部分的贡献会增加
总体上操作次数会增加 nums[right] - nums[mid]
移动左边界则会增加 -nums[left] + nums[mid]
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要瓶颈在排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxFrequencyScore(vector<int>& nums, long long k) 
    {
        sort(nums.begin(), nums.end());
        int result = 0, left = 0, n = nums.size();
        long long pre = 0L;
        for (int right = 0; right < n; right++) 
        {
            pre += nums[right] - nums[left + right >> 1];
            while (pre > k) pre += nums[left] - nums[left++ + right + 1 >> 1];
            result = max(result, right - left + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxFrequencyScore(int[] nums, long k) {
        Arrays.sort(nums);
        int result = 0, left = 0, n = nums.length;
        long pre = 0L;
        for (int right = 0; right < n; right++) {
            pre += nums[right] - nums[left + right >> 1];
            while (pre > k) pre += nums[left] - nums[left++ + right + 1 >> 1];
            result = Math.max(result, right - left + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxFrequencyScore(self, nums: List[int], k: int) -> int:
        nums.sort()
        n, pre, left, result = len(nums), list(accumulate(nums, initial=0)), 0, 0
        for right in range(n):
            while nums[mid := left + right >> 1] * (mid - left) - pre[mid] + pre[left] + pre[right + 1] - pre[mid + 1] - nums[mid] * (right - mid) > k:
                left += 1
            result = max(result, right - left + 1)
        return result
```
