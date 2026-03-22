# 2321 Maximum Score Of Spliced Array 拼接数组的最大分数

__Description:__

You are given two __0-indexed__ integer arrays `nums1` and `nums2`, both of length `n`.

You can choose two integers `left` and `right` where `0 <= left <= right < n` and __swap__ the subarray `nums1[left...right]` with the subarray `nums2[left...right]`.

- For example, if `nums1 = [1,2,3,4,5]` and `nums2 = [11,12,13,14,15]` and you choose `left = 1` and `right = 2`, `nums1` becomes [1,__12,13__,4,5] and `nums2` becomes [11,__2,3__,14,15].

You may choose to apply the mentioned operation once or not do anything.

The __score__ of the arrays is the __maximum__ of `sum(nums1)` and `sum(nums2)`, where `sum(arr)` is the sum of all the elements in the array `arr`.

Return the maximum possible score.

A __subarray__ is a contiguous sequence of elements within an array. `arr[left...right]` denotes the subarray that contains the elements of `nums` between indices `left` and `right` (__inclusive__).

__Example:__

Example 1:

```text
Input: nums1 = [60,60,60], nums2 = [10,90,10]
Output: 210
Explanation: Choosing left = 1 and right = 1, we have nums1 = [60,90,60] and nums2 = [10,60,10].
The score is max(sum(nums1), sum(nums2)) = max(210, 80) = 210.
```

Example 2:

```text
Input: nums1 = [20,40,20,70,30], nums2 = [50,20,50,40,20]
Output: 220
Explanation: Choosing left = 3, right = 4, we have nums1 = [20,40,20,40,20] and nums2 = [50,20,50,70,30].
The score is max(sum(nums1), sum(nums2)) = max(140, 220) = 220.
```

Example 3:

```text
Input: nums1 = [7,11,13], nums2 = [1,1,1]
Output: 31
Explanation: We choose not to swap any subarray.
The score is max(sum(nums1), sum(nums2)) = max(31, 3) = 31.
```

__Constraints:__

- `n == nums1.length == nums2.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 10 ^ 4`

__题目描述:__

给你两个下标从 __0__ 开始的整数数组 `nums1` 和 `nums2` ，长度都是 `n` 。

你可以选择两个整数 `left` 和 `right` ，其中 `0 <= left <= right < n` ，接着 __交换__ 两个子数组 `nums1[left...right]` 和 `nums2[left...right]` 。

- 例如，设 `nums1 = [1,2,3,4,5]` 和 `nums2 = [11,12,13,14,15]` ，整数选择 `left = 1` 和 `right = 2`，那么 `nums1` 会变为 [1,___12_,_13___,4,5] 而 `nums2` 会变为 [11,___2,3___,14,15] 。

你可以选择执行上述操作 一次 或不执行任何操作。

数组的 __分数__ 取 `sum(nums1)` 和 `sum(nums2)` 中的最大值，其中 `sum(arr)` 是数组 `arr` 中所有元素之和。

返回 可能的最大分数 。

__子数组__ 是数组中连续的一个元素序列。 `arr[left...right]` 表示子数组包含 `nums` 中下标 `left` 和 `right` 之间的元素（含下标 `left` 和 `right` 对应元素）。

__示例:__

示例 1：

```text
输入：nums1 = [60,60,60], nums2 = [10,90,10]
输出：210
解释：选择 left = 1 和 right = 1 ，得到 nums1 = [60,90,60] 和 nums2 = [10,60,10] 。
分数为 max(sum(nums1), sum(nums2)) = max(210, 80) = 210 。
```

示例 2：

```text
输入：nums1 = [20,40,20,70,30], nums2 = [50,20,50,40,20]
输出：220
解释：选择 left = 3 和 right = 4 ，得到 nums1 = [20,40,20,40,20] 和 nums2 = [50,20,50,70,30] 。
分数为 max(sum(nums1), sum(nums2)) = max(140, 220) = 220 。
```

示例 3：

```text
输入：nums1 = [7,11,13], nums2 = [1,1,1]
输出：31
解释：选择不交换任何子数组。
分数为 max(sum(nums1), sum(nums2)) = max(31, 3) = 31 。
```

__提示：__

- `n == nums1.length == nums2.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 10 ^ 4`

__思路:__

```text
动态规划
假设交换完之后 nums1 的分数最大
则 result = nums1[0] + nums1[1] + ... + nums2[left] + nums2[left + 1] + ... + nums2[right] - nums1[left] - nums1[left + 1] - ... - nums1[right] + nums1[right + 1] + ... + nums1[n - 1]
即 result = nums1[0] + nums1[1] + ... + (nums2[left] - nums1[left]) + ... + (nums2[right] - nums1[right]) + ... + nums1[n - 1]
设 diff[i] = nums2[i] - nums1[i]
那么需要求出 diff 的最大子数组和, 子数组的长度可以为 0
使用动态规划求解最大子数组和即可
最后加上 nums1 的和就是最终的结果
result = sum(nums1) + max_subarray_sum(diff)
因为不能确定 nums1 的分数更大, 所以需要分别求出 nums1 和 nums2 的最大分数, 取最大值即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumsSplicedArray(vector<int>& nums1, vector<int>& nums2) 
    {
        return max(helper(nums1, nums2), helper(nums2, nums1));
    }
private:
    int helper(vector<int>& nums1, vector<int>& nums2)
    {
        int s = 0, cur = 0;
        for (int i = 0, n = nums1.size(); i < n; i++) 
        {
            if ((s = s + nums2[i] - nums1[i]) < 0) s = 0;
            if (s > cur) cur = s;
        }
        return accumulate(nums1.begin(), nums1.end(), 0) + cur;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumsSplicedArray(int[] nums1, int[] nums2) {
        return Math.max(helper(nums1, nums2), helper(nums2, nums1));
    }

    private int helper(int[] nums1, int[] nums2) {
        int s = 0, cur = 0;
        for (int i = 0, n = nums1.length; i < n; i++) {
            if ((s = s + nums2[i] - nums1[i]) < 0) s = 0;
            if (s > cur) cur = s;
        }
        return Arrays.stream(nums1).sum() + cur;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumsSplicedArray(self, nums1: List[int], nums2: List[int]) -> int:
        def helper(nums1: List[int], nums2: List[int]) -> int:
            cur = s = 0
            for x, y in zip(nums1, nums2):
                cur = max(cur, s := (0 if (s := s + y - x) < 0 else s))
            return sum(nums1) + cur
        return max(helper(nums1, nums2), helper(nums2, nums1))
```
