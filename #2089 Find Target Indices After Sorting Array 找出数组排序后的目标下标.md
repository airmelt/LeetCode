# 2089 Find Target Indices After Sorting Array 找出数组排序后的目标下标

__Description:__

You are given a __0-indexed__ integer array `nums` and a target element `target`.

A __target index__ is an index `i` such that `nums[i] == target`.

Return _a list of the target indices of_ `nums` after _sorting_ `nums` _in __non-decreasing__ order_. If there are no target indices, return _an __empty__ list_. The returned list must be sorted in __increasing__ order.

__Example:__

Example 1:

```text
Input: nums = [1,2,5,2,3], target = 2
Output: [1,2]
Explanation: After sorting, nums is [1,2,2,3,5].
The indices where nums[i] == 2 are 1 and 2.
```

Example 2:

```text
Input: nums = [1,2,5,2,3], target = 3
Output: [3]
Explanation: After sorting, nums is [1,2,2,3,5].
The index where nums[i] == 3 is 3.
```

Example 3:

```text
Input: nums = [1,2,5,2,3], target = 5
Output: [4]
Explanation: After sorting, nums is [1,2,2,3,5].
The index where nums[i] == 5 is 4.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i], target <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 以及一个目标元素 `target` 。

__目标下标__ 是一个满足 `nums[i] == target` 的下标 `i` 。

将 `nums` 按 __非递减__ 顺序排序后，返回由 `nums` 中目标下标组成的列表。如果不存在目标下标，返回一个 __空__ 列表。返回的列表必须按 __递增__ 顺序排列。

__示例:__

示例 1：

```text
输入：nums = [1,2,5,2,3], target = 2
输出：[1,2]
解释：排序后，nums 变为 [1,2,2,3,5] 。
满足 nums[i] == 2 的下标是 1 和 2 。
```

示例 2：

```text
输入：nums = [1,2,5,2,3], target = 3
输出：[3]
解释：排序后，nums 变为 [1,2,2,3,5] 。
满足 nums[i] == 3 的下标是 3 。
```

示例 3：

```text
输入：nums = [1,2,5,2,3], target = 5
输出：[4]
解释：排序后，nums 变为 [1,2,2,3,5] 。
满足 nums[i] == 5 的下标是 4 。
```

示例 4：

```text
输入：nums = [1,2,5,2,3], target = 4
输出：[]
解释：nums 中不含值为 4 的元素。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i], target <= 100`

__思路:__

```text
1. 排序
排序后可以使用二分找到起始位置和结束位置
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要开销为排序
2. 计数
统计比 target 的小的个数 less
和与 target 相等的数 equal
则 [less, less + equal) 区间内的都是 target
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> targetIndices(vector<int>& nums, int target) 
    {
        int less = 0, equal = 0;
        for (const auto& num : nums) 
        {
            less += num < target;
            equal += num == target;
        }
        vector<int> result;
        for (int i = less; i < less + equal; i++) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> targetIndices(int[] nums, int target) {
        int less = 0, equal = 0;
        for (int num : nums) {
            if (num < target) ++less;
            else if (num == target) ++equal;
        }
        List<Integer> result = new ArrayList<>();
        for (int i = less; i < less + equal; i++) result.add(i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def targetIndices(self, nums: List[int], target: int) -> List[int]:
        return [i for i, val in enumerate(sorted(nums)) if val == target]
```
