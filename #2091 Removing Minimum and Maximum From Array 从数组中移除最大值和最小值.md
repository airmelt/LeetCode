# 2091 Removing Minimum and Maximum From Array 从数组中移除最大值和最小值

__Description:__

You are given a __0-indexed__ array of __distinct__ integers `nums`.

There is an element in `nums` that has the __lowest__ value and an element that has the __highest__ value. We call them the __minimum__ and __maximum__ respectively. Your goal is to remove __both__ these elements from the array.

A deletion is defined as either removing an element from the front of the array or removing an element from the back of the array.

Return the minimum number of deletions it would take to remove both the minimum and maximum element from the array.

__Example:__

Example 1:

```text
Input: nums = [2,10,7,5,4,1,8,6]
Output: 5
Explanation: 
The minimum element in the array is nums[5], which is 1.
The maximum element in the array is nums[1], which is 10.
We can remove both the minimum and maximum by removing 2 elements from the front and 3 elements from the back.
This results in 2 + 3 = 5 deletions, which is the minimum number possible.
```

Example 2:

```text
Input: nums = [0,-4,19,1,8,-2,-3,5]
Output: 3
Explanation: 
The minimum element in the array is nums[1], which is -4.
The maximum element in the array is nums[2], which is 19.
We can remove both the minimum and maximum by removing 3 elements from the front.
This results in only 3 deletions, which is the minimum number possible.
```

Example 3:

```text
Input: nums = [101]
Output: 1
Explanation:  
There is only one element in the array, which makes it both the minimum and maximum element.
We can remove it with 1 deletion.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`
- The integers in `nums` are __distinct__.

__题目描述:__

给你一个下标从 __0__ 开始的数组 `nums` ，数组由若干 __互不相同__ 的整数组成。

`nums` 中有一个值最小的元素和一个值最大的元素。分别称为 __最小值__ 和 __最大值__ 。你的目标是从数组中移除这两个元素。

一次 删除 操作定义为从数组的 前面 移除一个元素或从数组的 后面 移除一个元素。

返回将数组中最小值和最大值 都 移除需要的最小删除次数。

__示例:__

示例 1：

```text
输入：nums = [2,10,7,5,4,1,8,6]
输出：5
解释：
数组中的最小元素是 nums[5] ，值为 1 。
数组中的最大元素是 nums[1] ，值为 10 。
将最大值和最小值都移除需要从数组前面移除 2 个元素，从数组后面移除 3 个元素。
结果是 2 + 3 = 5 ，这是所有可能情况中的最小删除次数。
```

示例 2：

```text
输入：nums = [0,-4,19,1,8,-2,-3,5]
输出：3
解释：
数组中的最小元素是 nums[1] ，值为 -4 。
数组中的最大元素是 nums[2] ，值为 19 。
将最大值和最小值都移除需要从数组前面移除 3 个元素。
结果是 3 ，这是所有可能情况中的最小删除次数。
```

示例 3：

```text
输入：nums = [101]
输出：1
解释：
数组中只有这一个元素，那么它既是数组中的最小值又是数组中的最大值。
移除它只需要 1 次删除操作。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`
- `nums` 中的整数 __互不相同__

__思路:__

```text
模拟
找到数组的最大值对应的下标 min_idx, 最小值对应的下标 max_idx
设这两个数的较小值为 left, 较大值为 right
要么移除了 left 之后的元素, 要么移除了 right 之前的元素
要么同时移除了 left 之前的元素和 right 之后的元素
返回这三种情况的最小值即可
即 min({n - left, right + 1, left + 1 + n - right})
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumDeletions(vector<int>& nums) 
    {
        return min({max(min_element(nums.begin(), nums.end()) - nums.begin(), max_element(nums.begin(), nums.end()) - nums.begin()) + 1, (int)nums.size() - min(min_element(nums.begin(), nums.end()) - nums.begin(), max_element(nums.begin(), nums.end()) - nums.begin()), min(min_element(nums.begin(), nums.end()) - nums.begin(), max_element(nums.begin(), nums.end()) - nums.begin()) + 1 + (int)nums.size() - max(min_element(nums.begin(), nums.end()) - nums.begin(), max_element(nums.begin(), nums.end()) - nums.begin())});
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumDeletions(int[] nums) {
        int minIdx = 0, maxIdx = 0, n = nums.length;
        for (int i = 1; i < n; i++) {
            if (nums[i] < nums[minIdx]) minIdx = i;
            if (nums[i] > nums[maxIdx]) maxIdx = i;
        }
        return Math.min(Math.min(Math.max(maxIdx, minIdx) + 1, n - Math.min(maxIdx, minIdx)), Math.min(maxIdx, minIdx) + 1 + n - Math.max(maxIdx, minIdx));
    }
}
```

__Python__:

```Python
class Solution:
    def minimumDeletions(self, nums: List[int]) -> int:
        return min(max(nums.index(min(nums)), nums.index(max(nums))) + 1, len(nums) - min(nums.index(min(nums)), nums.index(max(nums))), min(nums.index(min(nums)), nums.index(max(nums))) + 1 + len(nums) - max(nums.index(min(nums)), nums.index(max(nums))))
```
