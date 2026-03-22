# 2799 Count Complete Subarrays in an Array 统计完全子数组的数目

__Description:__

You are given an array `nums` consisting of __positive__ integers.

We call a subarray of an array complete if the following condition is satisfied:

- The number of __distinct__ elements in the subarray is equal to the number of distinct elements in the whole array.

Return the number of complete subarrays.

A subarray is a contiguous non-empty part of an array.

__Example:__

Example 1:

```text
Input: nums = [1,3,1,2,2]
Output: 4
Explanation: The complete subarrays are the following: [1,3,1,2], [1,3,1,2,2], [3,1,2] and [3,1,2,2].
```

Example 2:

```text
Input: nums = [5,5,5,5]
Output: 10
Explanation: The array consists only of the integer 5, so any subarray is complete. The number of subarrays that we can choose is 10.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 2000`

__题目描述:__

给你一个由 __正__ 整数组成的数组 `nums` 。

如果数组中的某个子数组满足下述条件，则称之为 完全子数组 ：

- 子数组中 __不同__ 元素的数目等于整个数组不同元素的数目。

返回数组中 完全子数组 的数目。

子数组 是数组中的一个连续非空序列。

__示例:__

示例 1：

```text
输入：nums = [1,3,1,2,2]
输出：4
解释：完全子数组有：[1,3,1,2]、[1,3,1,2,2]、[3,1,2] 和 [3,1,2,2] 。
```

示例 2：

```text
输入：nums = [5,5,5,5]
输出：10
解释：数组仅由整数 5 组成，所以任意子数组都满足完全子数组的条件。子数组的总数为 10 。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 2000`

__思路:__

```text
滑动窗口
只需要找到窗口的右端点
再找到此时左端点最远能到达的位置 left
那么 left, left - 1, left - 2, ..., 0 都满足
所以累加上 left 即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countCompleteSubarrays(vector<int>& nums) 
    {
        int left = 0, result = 0, target = (unordered_set<int>(nums.begin(), nums.end())).size(), out = 0;
        unordered_map<int, int> cur;
        for (const auto& num : nums) 
        {
            ++cur[num];
            while (cur.size() == target) if (!--cur[(out = nums[left++])]) cur.erase(out);
            result += left;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countCompleteSubarrays(int[] nums) {
        int left = 0, result = 0, target = (new HashSet<Integer>(Arrays.stream(nums).boxed().collect(Collectors.toList()))).size(), out = 0;
        var cur = new HashMap<Integer, Integer>();
        for (int num : nums) {
            cur.merge(num, 1, Integer::sum);
            while (cur.size() == target) if (cur.merge(out = nums[left++], -1, Integer::sum) == 0) cur.remove(out);
            result += left;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countCompleteSubarrays(self, nums: List[int]) -> int:
        target, left, result, cur = len(set(nums)), 0, 0, defaultdict(int)
        for num in nums:
            cur[num] += 1
            while len(cur) == target:
                cur[out := nums[left]] -= 1
                if not cur[out]:
                    del cur[out]
                left += 1
            result += left
        return result
```
