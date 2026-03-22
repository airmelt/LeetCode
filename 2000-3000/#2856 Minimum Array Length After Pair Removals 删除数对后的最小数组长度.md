# 2856 Minimum Array Length After Pair Removals 删除数对后的最小数组长度

__Description:__

Given an integer array `num` sorted in non-decreasing order.

You can perform the following operation any number of times:

- Choose __two__ indices, `i` and `j`, where `nums[i] < nums[j]`.
- Then, remove the elements at indices `i` and `j` from `nums`. The remaining elements retain their original order, and the array is re-indexed.

Return the __minimum__ length of `nums` after applying the operation zero or more times.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4]

Output: 0

Explanation:
```

![2856-1](https://assets.leetcode.com/uploads/2024/05/18/tcase1.gif)

Example 2:

```text
Input: nums = [1,1,2,2,3,3]

Output: 0

Explanation:
```

![2856-2](https://assets.leetcode.com/uploads/2024/05/19/tcase2.gif)

Example 3:

```text
Input: nums = [1000000000,1000000000]

Output: 2

Explanation:

Since both numbers are equal, they cannot be removed.
```

Example 4:

```text
Input: nums = [2,3,4,4,4]

Output: 1

Explanation:
```

![2856-3](https://assets.leetcode.com/uploads/2024/05/19/tcase3.gif)

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `nums` is sorted in __non-decreasing__ order.

__题目描述:__

给你一个下标从 __0__ 开始的 __非递减__ 整数数组 `nums` 。

你可以执行以下操作任意次：

- 选择 __两个__ 下标 `i` 和 `j` ，满足 `nums[i] < nums[j]` 。
- 将 `nums` 中下标在 `i` 和 `j` 处的元素删除。剩余元素按照原来的顺序组成新的数组，下标也重新从 __0__ 开始编号。

请你返回一个整数，表示执行以上操作任意次后（可以执行 __0__ 次）， `nums` 数组的 __最小__ 数组长度。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4]

输出：0

解释：
```

![2856-4](https://pic.leetcode.cn/1716779983-AHhkVn-tcase1.gif)

示例 2：

```text
输入：nums = [1,1,2,2,3,3]

输出：0

解释：
```

![2856-5](https://pic.leetcode.cn/1716779979-GyQhVf-tcase2.gif)

示例 3：

```text
输入：nums = [1000000000,1000000000]

输出：2

解释：

由于两个数字相等，不能删除它们。
```

示例 4：

```text
输入：nums = [2,3,4,4,4]

输出：1

解释：
```

![2856-6](https://pic.leetcode.cn/1716779940-qRRlHk-tcase3.gif)

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `nums` 是 __非递减__ 数组。

__思路:__

```text
贪心
只需要统计出现次数最多的元素
由于数组是有序的
出现最多的元素如果出现次数超过一半一定会出现在中间位置, 这时出现最多的元素会留下一部分没法消除
如果出现次数少于一半
那么一定可以两两配对消除, 这时只要数组长度为偶数就能消除完, 否则会剩下一个
只用返回上述两种情况的较大值即可
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minLengthAfterRemovals(vector<int>& nums) 
    {
        return max((int)((upper_bound(nums.begin(), nums.end(), nums[nums.size() >> 1]) - lower_bound(nums.begin(), nums.end(), nums[nums.size() >> 1])) << 1) - (int)nums.size(), (int)nums.size() & 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int minLengthAfterRemovals(List<Integer> nums) {
        return Math.max(((lowerBound(nums, nums.get(nums.size() >> 1) + 1) - lowerBound(nums, nums.get(nums.size() >> 1))) << 1) - nums.size(), nums.size() & 1);
    }

    private int lowerBound(List<Integer> nums, int target) {
        int left = -1, right = nums.size();
        while (left + 1 < right) {
            int mid = left + ((right - left) >> 1);
            if (nums.get(mid) >= target) right = mid;
            else left = mid;
        }
        return right;
    }
}
```

__Python__:

```Python
class Solution:
    def minLengthAfterRemovals(self, nums: List[int]) -> int:
        return max(((bisect_right(nums, nums[len(nums) >> 1]) - bisect_left(nums, nums[len(nums) >> 1])) << 1) - len(nums), len(nums) & 1)
```
