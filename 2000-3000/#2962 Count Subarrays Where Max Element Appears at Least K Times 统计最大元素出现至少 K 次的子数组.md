# 2962 Count Subarrays Where Max Element Appears at Least K Times 统计最大元素出现至少 K 次的子数组

__Description:__

You are given an integer array `nums` and a __positive__ integer `k`.

Return _the number of subarrays where the __maximum__ element of_ `nums` _appears __at least___ `k` _times in that subarray._

A subarray is a contiguous sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,3,2,3,3], k = 2
Output: 6
Explanation: The subarrays that contain the element 3 at least 2 times are: [1,3,2,3], [1,3,2,3,3], [3,2,3], [3,2,3,3], [2,3,3] and [3,3].
```

Example 2:

```text
Input: nums = [1,4,2,1], k = 3
Output: 0
Explanation: No subarray contains the element 4 at least 3 times.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`
- `1 <= k <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` 和一个 __正整数__ `k` 。

请你统计有多少满足 「 `nums` 中的 __最大__ 元素」至少出现 `k` 次的子数组，并返回满足这一条件的子数组的数目。

子数组是数组中的一个连续元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,3,2,3,3], k = 2
输出：6
解释：包含元素 3 至少 2 次的子数组为：[1,3,2,3]、[1,3,2,3,3]、[3,2,3]、[3,2,3,3]、[2,3,3] 和 [3,3] 。
```

示例 2：

```text
输入：nums = [1,4,2,1], k = 3
输出：0
解释：没有子数组包含元素 4 至少 3 次。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`
- `1 <= k <= 10 ^ 5`

__思路:__

```text
滑动窗口
注意这个最大值是数组的最大值不是滑动窗口内的最大值
先找到数组的最大值
遍历数组累计最大值的计数 cur
如果最大值的计数等于 k
向右移动 left 直到窗口内的最大值的个数小于 k
此时 [left, right] 是恰好不满足有 k 个最大值的窗口的
说明 [0, left - 1] 中的任何一个值与 right 都可以构成一个合法的窗口
这个区间长度为 left
结果累加上 left 即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countSubarrays(vector<int>& nums, int k) 
    {
        long long result = 0LL, left = 0LL, cur = 0LL, target = *max_element(nums.begin(), nums.end());
        for (const auto& num : nums) 
        {
            cur += num == target;
            while (cur == k) cur -= nums[left++] == target;
            result += left;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long countSubarrays(int[] nums, int k) {
        long result = 0L, left = 0L, cur = 0L;
        int target = Arrays.stream(nums).max().getAsInt();
        for (int num : nums) {
            if (num == target) ++cur;
            while (cur == k) if (nums[(int)left++] == target) --cur;
            result += left;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countSubarrays(self, nums: List[int], k: int) -> int:
        target, result, cur, left = max(nums), 0, 0, 0
        for num in nums:
            cur += num == target
            while cur == k:
                if nums[left] == target:
                    cur -= 1
                left += 1
            result += left
        return result
```
