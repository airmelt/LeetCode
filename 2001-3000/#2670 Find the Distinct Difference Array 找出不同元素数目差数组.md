# 2670 Find the Distinct Difference Array 找出不同元素数目差数组

__Description:__

You are given a __0-indexed__ array `nums` of length `n`.

The __distinct difference__ array of `nums` is an array `diff` of length `n` such that `diff[i]` is equal to the number of distinct elements in the suffix `nums[i + 1, ..., n - 1]` __subtracted from__ the number of distinct elements in the prefix `nums[0, ..., i]`.

Return _the __distinct difference__ array of_ `nums`.

Note that `nums[i, ..., j]` denotes the subarray of `nums` starting at index `i` and ending at index `j` inclusive. Particularly, if `i > j` then `nums[i, ..., j]` denotes an empty subarray.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4,5]
Output: [-3,-1,1,3,5]
Explanation: For index i = 0, there is 1 element in the prefix and 4 distinct elements in the suffix. Thus, diff[0] = 1 - 4 = -3.
For index i = 1, there are 2 distinct elements in the prefix and 3 distinct elements in the suffix. Thus, diff[1] = 2 - 3 = -1.
For index i = 2, there are 3 distinct elements in the prefix and 2 distinct elements in the suffix. Thus, diff[2] = 3 - 2 = 1.
For index i = 3, there are 4 distinct elements in the prefix and 1 distinct element in the suffix. Thus, diff[3] = 4 - 1 = 3.
For index i = 4, there are 5 distinct elements in the prefix and no elements in the suffix. Thus, diff[4] = 5 - 0 = 5.
```

Example 2:

```text
Input: nums = [3,2,3,4,2]
Output: [-2,-1,0,2,3]
Explanation: For index i = 0, there is 1 element in the prefix and 3 distinct elements in the suffix. Thus, diff[0] = 1 - 3 = -2.
For index i = 1, there are 2 distinct elements in the prefix and 3 distinct elements in the suffix. Thus, diff[1] = 2 - 3 = -1.
For index i = 2, there are 2 distinct elements in the prefix and 2 distinct elements in the suffix. Thus, diff[2] = 2 - 2 = 0.
For index i = 3, there are 3 distinct elements in the prefix and 1 distinct element in the suffix. Thus, diff[3] = 3 - 1 = 2.
For index i = 4, there are 3 distinct elements in the prefix and no elements in the suffix. Thus, diff[4] = 3 - 0 = 3.
```

__Constraints:__

- `1 <= n == nums.length <= 50`
- `1 <= nums[i] <= 50`

__题目描述:__

给你一个下标从 __0__ 开始的数组 `nums` ，数组长度为 `n` 。

`nums` 的 __不同元素数目差__ 数组可以用一个长度为 `n` 的数组 `diff` 表示，其中 `diff[i]` 等于前缀 `nums[0, ..., i]` 中不同元素的数目 __减去__ 后缀 `nums[i + 1, ..., n - 1]` 中不同元素的数目。

返回 `nums` 的 __不同元素数目差__ 数组。

注意 `nums[i, ..., j]` 表示 `nums` 的一个从下标 `i` 开始到下标 `j` 结束的子数组（包含下标 `i` 和 `j` 对应元素）。特别需要说明的是，如果 `i > j` ，则 `nums[i, ..., j]` 表示一个空子数组。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4,5]
输出：[-3,-1,1,3,5]
解释：
对于 i = 0，前缀中有 1 个不同的元素，而在后缀中有 4 个不同的元素。因此，diff[0] = 1 - 4 = -3 。
对于 i = 1，前缀中有 2 个不同的元素，而在后缀中有 3 个不同的元素。因此，diff[1] = 2 - 3 = -1 。
对于 i = 2，前缀中有 3 个不同的元素，而在后缀中有 2 个不同的元素。因此，diff[2] = 3 - 2 = 1 。
对于 i = 3，前缀中有 4 个不同的元素，而在后缀中有 1 个不同的元素。因此，diff[3] = 4 - 1 = 3 。
对于 i = 4，前缀中有 5 个不同的元素，而在后缀中有 0 个不同的元素。因此，diff[4] = 5 - 0 = 5 。
```

示例 2：

```text
输入：nums = [3,2,3,4,2]
输出：[-2,-1,0,2,3]
解释：
对于 i = 0，前缀中有 1 个不同的元素，而在后缀中有 3 个不同的元素。因此，diff[0] = 1 - 3 = -2 。
对于 i = 1，前缀中有 2 个不同的元素，而在后缀中有 3 个不同的元素。因此，diff[1] = 2 - 3 = -1 。
对于 i = 2，前缀中有 2 个不同的元素，而在后缀中有 2 个不同的元素。因此，diff[2] = 2 - 2 = 0 。
对于 i = 3，前缀中有 3 个不同的元素，而在后缀中有 1 个不同的元素。因此，diff[3] = 3 - 1 = 2 。
对于 i = 4，前缀中有 3 个不同的元素，而在后缀中有 0 个不同的元素。因此，diff[4] = 3 - 0 = 3 。
```

__提示：__

- `1 <= n == nums.length <= 50`
- `1 <= nums[i] <= 50`

__思路:__

```text
模拟
可以先从后往前遍历记录后缀中的不同元素数目
再从前往后遍历记录前缀中的不同元素数目
计算两者的差值即可
也可以一次遍历, 使用两个哈希表, 双指针记录
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> distinctDifferenceArray(vector<int>& nums) 
    {
        unordered_set<int> st1, st2;
        int n = nums.size();
        vector<int> result(n);
        for (int i = 0, j = n - 1; i < n; i++, j--) 
        {
            st1.insert(nums[i]);
            result[i] += st1.size(),
            result[j] -= st2.size();
            st2.insert(nums[j]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] distinctDifferenceArray(int[] nums) {
        int n = nums.length, suf[] = new int[n + 1], result[] = new int[n];
        var s = new HashSet<Integer>();
        for (int i = n - 1; i > 0; i--) {
            s.add(nums[i]);
            suf[i] = s.size();
        }
        s.clear();
        for (int i = 0; i < n; i++) {
            s.add(nums[i]);
            result[i] = s.size() - suf[i + 1];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def distinctDifferenceArray(self, nums: List[int]) -> List[int]:
        return [len(set(nums[:i + 1])) - len(set(nums[i + 1:])) for i in range(len(nums))]
```
