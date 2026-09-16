# 3171 Find Subarray With Bitwise OR Closest to K 找到按位或最接近 K 的子数组

__Description:__

You are given an array `nums` and an integer `k`. You need to find a __subarray__ of `nums` such that the __absolute difference__ between `k` and the bitwise `OR` of the subarray elements is as __small__ as possible. In other words, select a subarray `nums[l..r]` such that `|k - (nums[l] OR nums[l + 1] ... OR nums[r])|` is minimum.

Return the __minimum__ possible value of the absolute difference.

A __subarray__ is a contiguous __non-empty__ sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,2,4,5], k = 3

Output: 0

Explanation:

The subarray nums[0..1] has OR value 3, which gives the minimum absolute difference |3 - 3| = 0.
```

Example 2:

```text
Input: nums = [1,3,1,3], k = 2

Output: 1

Explanation:

The subarray nums[1..1] has OR value 3, which gives the minimum absolute difference |3 - 2| = 1.
```

Example 3:

```text
Input: nums = [1], k = 10

Output: 9

Explanation:

There is a single subarray with OR value 1, which gives the minimum absolute difference |10 - 1| = 9.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= s <= 10 ^ 9`

__题目描述:__

给你一个数组 `nums` 和一个整数 `k` 。你需要找到 `nums` 的一个 __子数组__ ，满足子数组中所有元素按位或运算 `OR` 的值与 `k` 的 __绝对差__ 尽可能 __小__ 。换言之，你需要选择一个子数组 `nums[l..r]` 满足 `|k - (nums[l] OR nums[l + 1] ... OR nums[r])|` 最小。

请你返回 __最小__ 的绝对差值。

__子数组__ 是数组中连续的 __非空__ 元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,2,4,5], k = 3

输出：0

解释：

子数组 nums[0..1] 的按位 OR 运算值为 3 ，得到最小差值 |3 - 3| = 0 。
```

示例 2：

```text
输入：nums = [1,3,1,3], k = 2

输出：1

解释：

子数组 nums[1..1] 的按位 OR 运算值为 3 ，得到最小差值 |3 - 2| = 1 。
```

示例 3：

```text
输入：nums = [1], k = 10

输出：9

解释：

只有一个子数组，按位 OR 运算值为 1 ，得到最小差值 |10 - 1| = 9 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= s <= 10 ^ 9`

__思路:__

```text
1. 位运算
每次遍历的时候把当前数并入之前的数
遇到是子集的时候就停止
时间复杂度为 O(NlogM), 空间复杂度为 O(1), 其中 M 为数组中的最大值
2. 滑动窗口
子数组可以考虑使用滑动窗口
要解决滑动窗口移除元素的问题
OR 的逆运算为 XOR
所以需要记录上一段窗口的 OR
可以直接记录在 nums 数组中
倒序遍历, nums[i] |= nums[i + 1] 即可
当窗口中的数比 k 大时, 继续 OR 只会更大
这时就需要移动左边界直到窗口里的 OR 比 k 小
记录上一段窗口的 OR 于 nums[left]
重置上一段结束的窗口边界于 bottom, 重置 right_or 为 0
如果 left 比 right 小, 此时窗口中的值已经比 k 小了
还需要再更新一下结果保证比 k 小的部分也得到更新
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumDifference(vector<int>& nums, int k) 
    {
        int result = INT_MAX;
        for (int i = 0, n = nums.size(), num = 0; i < n; i++) 
        {
            result = min(result, abs((num = nums[i]) - k));
            for (int j = i - 1; j > -1 and (nums[j] | num) != nums[j]; j--) result = min(result, abs((nums[j] |= num) - k));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumDifference(int[] nums, int k) {
        int result = Integer.MAX_VALUE;
        for (int i = 0, n = nums.length, num = 0; i < n; i++) {
            result = Math.min(result, Math.abs((num = nums[i]) - k));
            for (int j = i - 1; j > -1 && (nums[j] | num) != nums[j]; j--) result = Math.min(result, Math.abs((nums[j] |= num) - k));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumDifference(self, nums: List[int], k: int) -> int:
        result, bottom, left, right_or = inf, 0, 0, 0
        for right, num in enumerate(nums):
            right_or |= num
            while left <= right and (window := nums[left] | right_or) > k:
                result = min(result, window - k)
                left += 1
                if left > bottom:
                    for i in range(right - 1, left - 1, -1):
                        nums[i] |= nums[i + 1]
                    bottom, right_or = right, 0
            if left <= right:
                result = min(result, k - (nums[left] | right_or))
        return result
```
