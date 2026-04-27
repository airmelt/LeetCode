# 2779 Maximum Beauty of an Array After Applying Operation 数组的最大美丽值

__Description:__

You are given a __0-indexed__ array `nums` and a __non-negative__ integer `k`.

In one operation, you can do the following:

- Choose an index `i` that __hasn't been chosen before__ from the range `[0, nums.length - 1]`.
- Replace `nums[i]` with any integer from the range `[nums[i] - k, nums[i] + k]`.

The beauty of the array is the length of the longest subsequence consisting of equal elements.

Return _the __maximum__ possible beauty of the array_ `nums` _after applying the operation any number of times._

Note that you can apply the operation to each index only once.

A subsequence of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the order of the remaining elements.

__Example:__

Example 1:

```text
Input: nums = [4,6,1,2], k = 2
Output: 3
Explanation: In this example, we apply the following operations:
```

- Choose index 1, replace it with 4 (from range [4,8]), nums = [4,4,1,2].
- Choose index 3, replace it with 4 (from range [0,4]), nums = [4,4,1,4].
After the applied operations, the beauty of the array nums is 3 (subsequence consisting of indices 0, 1, and 3).

It can be proven that 3 is the maximum possible length we can achieve.

Example 2:

```text
Input: nums = [1,1,1,1], k = 10
Output: 4
Explanation: In this example we don't have to apply any operations.
The beauty of the array nums is 4 (whole array).
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i], k <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个 __非负__ 整数 `k` 。

在一步操作中，你可以执行下述指令：

- 在范围 `[0, nums.length - 1]` 中选择一个 __此前没有选过__ 的下标 `i` 。
- 将 `nums[i]` 替换为范围 `[nums[i] - k, nums[i] + k]` 内的任一整数。

数组的 美丽值 定义为数组中由相等元素组成的最长子序列的长度。

对数组 `nums` 执行上述操作任意次后，返回数组可能取得的 __最大__ 美丽值。

注意：你 只 能对每个下标执行 一次 此操作。

数组的 子序列 定义是：经由原数组删除一些元素（也可能不删除）得到的一个新数组，且在此过程中剩余元素的顺序不发生改变。

__示例:__

```text
示例 1：

输入：nums = [4,6,1,2], k = 2
输出：3
解释：在这个示例中，我们执行下述操作：
```

- 选择下标 1 ，将其替换为 4（从范围 [4,8] 中选出），此时 nums = [4,4,1,2] 。
- 选择下标 3 ，将其替换为 4（从范围 [0,4] 中选出），此时 nums = [4,4,1,4] 。

执行上述操作后，数组的美丽值是 3（子序列由下标 0 、1 、3 对应的元素组成）。
可以证明 3 是我们可以得到的由相等元素组成的最长子序列长度。

```text
示例 2：

输入：nums = [1,1,1,1], k = 10
输出：4
解释：在这个示例中，我们无需执行任何操作。
数组 nums 的美丽值是 4（整个数组）。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i], k <= 10 ^ 5`

__思路:__

```text
排序 ➕ 滑动窗口
由于只用取子序列
所以和数组原来的顺序没有关系
先排序
然后保证窗口里的值不超过 2k 即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumBeauty(vector<int>& nums, int k) 
    {
        int left = 0, result = 0, n = nums.size();
        sort(nums.begin(), nums.end());
        for (int right = 0; right < n; right++) 
        {
            while (nums[right] - nums[left] > (k << 1)) ++left;
            result = max(right - left + 1, result);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumBeauty(int[] nums, int k) {
        int left = 0, result = 0, n = nums.length;
        Arrays.sort(nums);
        for (int right = 0; right < n; right++) {
            while (nums[right] - nums[left] > (k << 1)) ++left;
            result = Math.max(right - left + 1, result);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumBeauty(self, nums: List[int], k: int) -> int:
        left, result, nums = 0, 0, sorted(nums)
        for right, num in enumerate(nums):
            while num - nums[left] > (k << 1):
                left += 1
            result = max(right - left + 1, result)
        return result
```
