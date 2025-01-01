# 2395 Find Subarrays With Equal Sum 和相等的子数组

__Description:__

Given a __0-indexed__ integer array `nums`, determine whether there exist __two__ subarrays of length `2` with __equal__ sum. Note that the two subarrays must begin at __different__ indices.

Return `true` _if these subarrays exist, and_ `false` _otherwise._

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [4,2,4]
Output: true
Explanation: The subarrays with elements [4,2] and [2,4] have the same sum of 6.
```

Example 2:

```text
Input: nums = [1,2,3,4,5]
Output: false
Explanation: No two subarrays of size 2 have the same sum.
```

Example 3:

```text
Input: nums = [0,0,0]
Output: true
Explanation: The subarrays [nums[0],nums[1]] and [nums[1],nums[2]] have the same sum of 0. 
Note that even though the subarrays have the same content, the two subarrays are considered different because they are in different positions in the original array.
```

__Constraints:__

- `2 <= nums.length <= 1000`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，判断是否存在 __两个__ 长度为 `2` 的子数组且它们的 __和__ 相等。注意，这两个子数组起始位置的下标必须 __不相同__ 。

如果这样的子数组存在，请返回 `true`，否则返回 `false` 。

子数组 是一个数组中一段连续非空的元素组成的序列。

__示例:__

示例 1：

```text
输入：nums = [4,2,4]
输出：true
解释：元素为 [4,2] 和 [2,4] 的子数组有相同的和 6 。
```

示例 2：

```text
输入：nums = [1,2,3,4,5]
输出：false
解释：没有长度为 2 的两个子数组和相等。
```

示例 3：

```text
输入：nums = [0,0,0]
输出：true
解释：子数组 [nums[0],nums[1]] 和 [nums[1],nums[2]] 的和相等，都为 0 。
注意即使子数组的元素相同，这两个子数组也视为不相同的子数组，因为它们在原数组中的起始位置不同。
```

__提示：__

- `2 <= nums.length <= 1000`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__思路:__

```text
哈希表
记录相邻两个元素的和, 如果有相同的和, 则返回 true
否则返回 false
如果求 k 个数字可以使用滑动窗口或者前缀和
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool findSubarrays(vector<int>& nums) 
    {
        unordered_set<int> s;
        for (int i = 1, n = nums.size(); i < n; i++) if (!s.insert(nums[i - 1] + nums[i]).second) return true;
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean findSubarrays(int[] nums) {
        var set = new HashSet<Integer>();
        for (int i = 1, n = nums.length; i < n; i++) if (!set.add(nums[i - 1] + nums[i])) return true;
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def findSubarrays(self, nums: List[int]) -> bool:
        return len(set(map(sum, pairwise(nums)))) < len(nums) - 1
```
