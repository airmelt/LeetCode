# 2740 Find the Value of the Partition 找出分区值

__Description:__

You are given a __positive__ integer array `nums`.

Partition `nums` into two arrays, `nums1` and `nums2`, such that:

- Each element of the array `nums` belongs to either the array `nums1` or the array `nums2`.
- Both arrays are __non-empty__.
- The value of the partition is __minimized__.

The value of the partition is `|max(nums1) - min(nums2)|`.

Here, `max(nums1)` denotes the maximum element of the array `nums1`, and `min(nums2)` denotes the minimum element of the array `nums2`.

Return the integer denoting the value of such partition.

__Example:__

Example 1:

```text
Input: nums = [1,3,2,4]
Output: 1
Explanation: We can partition the array nums into nums1 = [1,2] and nums2 = [3,4].
```

- The maximum element of the array nums1 is equal to 2.
- The minimum element of the array nums2 is equal to 3.

The value of the partition is |2 - 3| = 1.
It can be proven that 1 is the minimum value out of all partitions.

Example 2:

```text
Input: nums = [100,1,10]
Output: 9
Explanation: We can partition the array nums into nums1 = [10] and nums2 = [100,1].
```

- The maximum element of the array nums1 is equal to 10.
- The minimum element of the array nums2 is equal to 1.

The value of the partition is |10 - 1| = 9.
It can be proven that 9 is the minimum value out of all partitions.

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个 __正__ 整数数组 `nums` 。

将 `nums` 分成两个数组: `nums1` 和 `nums2` ，并满足下述条件:

- 数组 `nums` 中的每个元素都属于数组 `nums1` 或数组 `nums2` 。
- 两个数组都 __非空__ 。
- 分区值 __最小__ 。

分区值的计算方法是 `|max(nums1) - min(nums2)|` 。

其中， `max(nums1)` 表示数组 `nums1` 中的最大元素， `min(nums2)` 表示数组 `nums2` 中的最小元素。

返回表示分区值的整数。

__示例:__

示例 1：

```text
输入：nums = [1,3,2,4]
输出：1
解释：可以将数组 nums 分成 nums1 = [1,2] 和 nums2 = [3,4] 。
```

- 数组 nums1 的最大值等于 2 。
- 数组 nums2 的最小值等于 3 。

分区值等于 |2 - 3| = 1 。
可以证明 1 是所有分区方案的最小值。

示例 2：

```text
输入：nums = [100,1,10]
输出：9
解释：可以将数组 nums 分成 nums1 = [10] 和 nums2 = [100,1] 。
```

- 数组 nums1 的最大值等于 10 。
- 数组 nums2 的最小值等于 1 。

分区值等于 |10 - 1| = 9 。
可以证明 9 是所有分区方案的最小值。

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
贪心
只要找到两个数的差最小
将数组排序
返回两个相邻数之间的差的最小值即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findValueOfPartition(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        int result = INT_MAX, n = nums.size();
        for (int i = 1; i < n; i++) result = min(nums[i] - nums[i - 1], result);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findValueOfPartition(int[] nums) {
        Arrays.sort(nums);
        int result = Integer.MAX_VALUE, n = nums.length;
        for (int i = 1; i < n; i++) result = Math.min(nums[i] - nums[i - 1], result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findValueOfPartition(self, nums: List[int]) -> int:
        return min(y - x for x, y in pairwise(sorted(nums)))
```
