# 3107 Minimum Operations to Make Median of Array Equal to K 使数组中位数等于 K 的最少操作数

__Description:__

You are given an integer array `nums` and a __non-negative__ integer `k`. In one operation, you can increase or decrease any element by 1.

Return the __minimum__ number of operations needed to make the __median__ of `nums` _equal_ to `k`.

The median of an array is defined as the middle element of the array when it is sorted in non-decreasing order. If there are two choices for a median, the larger of the two values is taken.

__Example:__

Example 1:

```text
Input: nums = [2,5,6,8,5], k = 4

Output: 2

Explanation:
```

We can subtract one from `nums[1]` and `nums[4]` to obtain `[2, 4, 6, 8, 4]`. The median of the resulting array is equal to `k`.

Example 2:

```text
Input: nums = [2,5,6,8,5], k = 7

Output: 3

Explanation:
```

We can add one to `nums[1]` twice and add one to `nums[2]` once to obtain `[2, 7, 7, 8, 5]`.

Example 3:

```text
Input: nums = [1,2,3,4,5,6], k = 4

Output: 0

Explanation:
```

The median of the array is already equal to `k`.

__Constraints:__

- `1 <= nums.length <= 2 * 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` 和一个 __非负__ 整数 `k` 。一次操作中，你可以选择任一元素 加 `1` 或者减 `1` 。

请你返回将 `nums` __中位数__ 变为 `k` 所需要的 __最少__ 操作次数。

一个数组的中位数指的是数组按非递减顺序排序后最中间的元素。如果数组长度为偶数，我们选择中间两个数的较大值为中位数。

__示例:__

示例 1：

```text
输入：nums = [2,5,6,8,5], k = 4

输出：2
```

_解释:_
我们将 `nums[1]` 和 `nums[4]` 减 `1` 得到 `[2, 4, 6, 8, 4]` 。现在数组的中位数等于 `k` 。

示例 2：

```text
输入：nums = [2,5,6,8,5], k = 7

输出：3
```

_解释:_
我们将 `nums[1]` 增加 1 两次，并且将 `nums[2]` 增加 1 一次，得到 `[2, 7, 7, 8, 5]` 。

示例 3：

```text
输入：nums = [1,2,3,4,5,6], k = 4

输出：0
```

_解释:_
数组中位数已经等于 `k` 了。

__提示：__

- `1 <= nums.length <= 2 * 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 9`

__思路:__

```text
排序
将数组排序后
那么需要将正中间的数 idx 改为 k
需要 abs(nums[idx] - k)
然后从 idx 出发, 后面的数都需要不小于 k
前面的数都需要不大于 k
遍历两次即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要瓶颈在排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minOperationsToMakeMedianK(vector<int>& nums, int k) 
    {
        sort(nums.begin(), nums.end());
        int n = nums.size(), idx = n >> 1;
        long long result = abs(nums[idx] - k);
        for (int i = idx + 1; i < n; i++) if (nums[i] < k) result += k - nums[i];
        for (int i = idx - 1; i > -1; i--) if (nums[i] > k) result += nums[i] - k;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long minOperationsToMakeMedianK(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length, idx = n >>> 1;
        long result = Math.abs(nums[idx] - k);
        for (int i = idx + 1; i < n; i++) if (nums[i] < k) result += k - nums[i];
        for (int i = idx - 1; i > -1; i--) if (nums[i] > k) result += nums[i] - k;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperationsToMakeMedianK(self, nums: List[int], k: int) -> int:
        return sum(nums[i] - k for i in range(idx - 1, -1, -1) if nums[i] > k) + sum(k - nums[i] for i in range(idx + 1, n) if nums[i] < k) + abs(nums[idx] - k) if (idx := (n := len(nums := sorted(nums))) >> 1) else abs(nums[idx] - k)
        nums.sort()
```
