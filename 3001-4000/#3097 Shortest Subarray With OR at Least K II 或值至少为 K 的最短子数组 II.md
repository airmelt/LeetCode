# 3097 Shortest Subarray With OR at Least K II 或值至少为 K 的最短子数组 II

__Description:__

You are given an array `nums` of __non-negative__ integers and an integer `k`.

An array is called __special__ if the bitwise `OR` of all of its elements is __at least__ `k`.

Return _the length of the __shortest__ __special__ __non-empty__ subarray of_ `nums`, _or return_ `-1` _if no special subarray exists_.

__Example:__

Example 1:

```text
Input: nums = [1,2,3], k = 2

Output: 1

Explanation:
```

The subarray `[3]` has `OR` value of `3`. Hence, we return `1`.

Example 2:

```text
Input: nums = [2,1,8], k = 10

Output: 3

Explanation:
```

The subarray `[2,1,8]` has `OR` value of `11`. Hence, we return `3`.

Example 3:

```text
Input: nums = [1,2], k = 0

Output: 1

Explanation:
```

The subarray `[1]` has `OR` value of `1`. Hence, we return `1`.

__Constraints:__

- `1 <= nums.length <= 2 * 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`
- `0 <= k <= 10 ^ 9`

__题目描述:__

给你一个 __非负__ 整数数组 `nums` 和一个整数 `k` 。

如果一个数组中所有元素的按位或运算 `OR` 的值 __至少__ 为 `k` ，那么我们称这个数组是 __特别的__ 。

请你返回 `nums` 中 __最短特别非空__ 子数组的长度，如果特别子数组不存在，那么返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [1,2,3], k = 2

输出：1

解释：
```

子数组 `[3]` 的按位 `OR` 值为 `3` ，所以我们返回 `1` 。

示例 2：

```text
输入：nums = [2,1,8], k = 10

输出：3

解释：
```

子数组 `[2,1,8]` 的按位 `OR` 值为 `11` ，所以我们返回 `3` 。

示例 3：

```text
输入：nums = [1,2], k = 0

输出：1

解释：
```

子数组 `[1]` 的按位 `OR` 值为 `1` ，所以我们返回 `1` 。

__提示：__

- `1 <= nums.length <= 2 * 10 ^ 5`
- `0 <= nums[i] <= 10 ^ <font size="1">9`
- `0 <= k <= 10 ^ 9`

__思路:__

```text
滑动窗口
记录 left 到 right 到所有或值的结果 pre
同时记录下上一个滑动窗口的右边界 start
如果滑动窗口的左边界 left 超过了 start
那就重新构造滑动窗口
使用类似前缀和的方式将 [left, right - 1] 构造为新的滑动窗口
start 记录为新的滑动窗口的右边界
清空滑动窗口里的或值
每次满足滑动窗口里的值不小于 k 就更新窗口
这样 left/right 和滑动窗口里的值都只会更新一次
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumSubarrayLength(vector<int>& nums, int k) 
    {
        int result = INT_MAX, left = 0, start = 0, pre = 0, n = nums.size();
        for (int right = 0; right < n; right++)
        {
            pre |= nums[right];
            while (left <= right and (nums[left] | pre) >= k)
            {
                result = min(result, right - left++ + 1);
                if (start < left) {
                    for (int i = right - 1; i >= left; i--) nums[i] |= nums[i + 1];
                    start = right;
                    pre = 0;
                }
            }
        }
        return result == INT_MAX ? -1 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumSubarrayLength(int[] nums, int k) {
        int inf = 0x3f3f3f3f, result = inf, n = nums.length;
        for (int i = 0, num = 0; i < n; i++) {
            if ((num = nums[i]) >= k) return 1;
            for (int j = i - 1; j > -1 && (nums[j] | num) != nums[j]; j--) if ((nums[j] |= num) >= k) result = Math.min(result, i - j + 1);
        }
        return result < inf ? result : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumSubarrayLength(self, nums: List[int], k: int) -> int:
        result = inf
        for i, num in enumerate(nums):
            if num >= k:
                return 1
            for j in range(i - 1, -1, -1):
                if nums[j] | num == nums[j]:
                    break
                nums[j] |= num
                if nums[j] >= k:
                    result = min(result, i - j + 1)
        return result if result < inf else -1
                
```
