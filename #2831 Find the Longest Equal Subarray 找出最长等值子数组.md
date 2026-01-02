# 2831 Find the Longest Equal Subarray 找出最长等值子数组

__Description:__

You are given a __0-indexed__ integer array `nums` and an integer `k`.

A subarray is called equal if all of its elements are equal. Note that the empty subarray is an equal subarray.

Return _the length of the __longest__ possible equal subarray after deleting __at most___ `k` _elements from_ `nums`.

A subarray is a contiguous, possibly empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,3,2,3,1,3], k = 3
Output: 3
Explanation: It's optimal to delete the elements at index 2 and index 4.
After deleting them, nums becomes equal to [1, 3, 3, 3].
The longest equal subarray starts at i = 1 and ends at j = 3 with length equal to 3.
It can be proven that no longer equal subarrays can be created.
```

Example 2:

```text
Input: nums = [1,1,2,2,1,1], k = 2
Output: 4
Explanation: It's optimal to delete the elements at index 2 and index 3.
After deleting them, nums becomes equal to [1, 1, 1, 1].
The array itself is an equal subarray, so the answer is 4.
It can be proven that no longer equal subarrays can be created.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= nums.length`
- `0 <= k <= nums.length`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `k` 。

如果子数组中所有元素都相等，则认为子数组是一个 等值子数组 。注意，空数组是 等值子数组 。

从 `nums` 中删除最多 `k` 个元素后，返回可能的最长等值子数组的长度。

子数组 是数组中一个连续且可能为空的元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,3,2,3,1,3], k = 3
输出：3
解释：最优的方案是删除下标 2 和下标 4 的元素。
删除后，nums 等于 [1, 3, 3, 3] 。
最长等值子数组从 i = 1 开始到 j = 3 结束，长度等于 3 。
可以证明无法创建更长的等值子数组。
```

示例 2：

```text
输入：nums = [1,1,2,2,1,1], k = 2
输出：4
解释：最优的方案是删除下标 2 和下标 3 的元素。 
删除后，nums 等于 [1, 1, 1, 1] 。 
数组自身就是等值子数组，长度等于 4 。 
可以证明无法创建更长的等值子数组。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= nums.length`
- `0 <= k <= nums.length`

__思路:__

```text
滑动窗口
将数组元素按值分组
那么只要计算每一组的 pos[right] - pos[left] + 1
就能得到子数组的长度
这里面有 right - left + 1 个元素是相等的不需要删除
那么需要删除的就是 pos[right] - pos[left] - right + left
即 pos[right] - right - (pos[left] + left)
如果这个数不大于 k 说明可以通过删除 k 个元素得到长度为 pos[right] - pos[left] + 1 的子数组
所以我们可以保存 pos[i] - i, 即 i 在 pos 中的下标简化计算
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestEqualSubarray(vector<int>& nums, int k) 
    {
        int n = nums.size(), result = 0;
        vector<vector<int>> group(n + 1);
        for (int i = 0; i < n; i++) group[nums[i]].emplace_back(i - group[nums[i]].size());
        for (const auto& v : group)
        {
            if (v.size() > result)
            {
                for (int left = 0, right = 0; right < v.size(); right++)
                {
                    while (v[right] - v[left] > k) ++left;
                    result = max(result, right - left + 1);
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestEqualSubarray(List<Integer> nums, int k) {
        int n = nums.size(), result = 0;
        List<Integer>[] group = new ArrayList[n + 1];
        Arrays.setAll(group, i -> new ArrayList<>());
        for (int i = 0; i < n; i++) group[nums.get(i)].add(i - group[nums.get(i)].size());
        for (var v : group) {
            if (v.size() > result) {
                for (int left = 0, right = 0; right < v.size(); right++) {
                    while (v.get(right) - v.get(left) > k) ++left;
                    result = Math.max(result, right - left + 1);
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestEqualSubarray(self, nums: List[int], k: int) -> int:
        group, result = defaultdict(list), 0
        for i, num in enumerate(nums):
            group[num].append(i - len(group[num]))
        for v in group.values():
            if len(v) > result:
                left = 0
                for right, p in enumerate(v):
                    while p - v[left] > k:
                        left += 1
                    result = max(result, right - left + 1)
        return result
```
