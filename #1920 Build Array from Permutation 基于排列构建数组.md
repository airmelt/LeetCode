# 1920 Build Array from Permutation 基于排列构建数组

__Description:__

Given a __zero-based permutation__ `nums` (__0-indexed__), build an array `ans` of the __same length__ where `ans[i] = nums[nums[i]]` for each `0 <= i < nums.length` and return it.

A __zero-based permutation__ `nums` is an array of __distinct__ integers from `0` to `nums.length - 1` (__inclusive__).

__Example:__

Example 1:

```text
Input: nums = [0,2,1,5,3,4]
Output: [0,1,2,4,5,3]
Explanation: The array ans is built as follows: 
ans = [nums[nums[0]], nums[nums[1]], nums[nums[2]], nums[nums[3]], nums[nums[4]], nums[nums[5]]]
    = [nums[0], nums[2], nums[1], nums[5], nums[3], nums[4]]
    = [0,1,2,4,5,3]
```

Example 2:

```text
Input: nums = [5,0,1,2,3,4]
Output: [4,5,0,1,2,3]
Explanation: The array ans is built as follows:
ans = [nums[nums[0]], nums[nums[1]], nums[nums[2]], nums[nums[3]], nums[nums[4]], nums[nums[5]]]
    = [nums[5], nums[0], nums[1], nums[2], nums[3], nums[4]]
    = [4,5,0,1,2,3]
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `0 <= nums[i] < nums.length`
- The elements in `nums` are __distinct__.

__Follow-up:__ Can you solve it without using an extra space (i.e., `O(1)` memory)?

__题目描述:__

给你一个 __从 0 开始的排列__ `nums`（__下标也从 0 开始__）。请你构建一个 __同样长度__ 的数组 `ans` ，其中，对于每个 `i`（ `0 <= i < nums.length`），都满足 `ans[i] = nums[nums[i]]` 。返回构建好的数组 `ans` 。

__从 0 开始的排列__ `nums` 是一个由 `0` 到 `nums.length - 1`（ `0` 和 `nums.length - 1` 也包含在内）的不同整数组成的数组。

__示例:__

示例 1：

```text
输入：nums = [0,2,1,5,3,4]
输出：[0,1,2,4,5,3]
解释：数组 ans 构建如下：
ans = [nums[nums[0]], nums[nums[1]], nums[nums[2]], nums[nums[3]], nums[nums[4]], nums[nums[5]]]
    = [nums[0], nums[2], nums[1], nums[5], nums[3], nums[4]]
    = [0,1,2,4,5,3]
```

示例 2：

```text
输入：nums = [5,0,1,2,3,4]
输出：[4,5,0,1,2,3]
解释：数组 ans 构建如下：
ans = [nums[nums[0]], nums[nums[1]], nums[nums[2]], nums[nums[3]], nums[nums[4]], nums[nums[5]]]
    = [nums[5], nums[0], nums[1], nums[2], nums[3], nums[4]]
    = [4,5,0,1,2,3]
```

__提示：__

- `1 <= nums.length <= 1000`
- `0 <= nums[i] < nums.length`
- `nums` 中的元素 __互不相同__

__思路:__

```text
1. 暴力法
直接将 nums[nums[i]] 赋值给 result[i]
时间复杂度为 O(N), 空间复杂度为 O(N), 若不计输出空间则为 O(1)
2. 原地修改
将 nums[i] 修改为 nums[i] + C * (nums[nums[i]] % C), C 为一个足够大的数, 此题中 C = 1000, 即用高位存储 nums[nums[i]]
然后再将 nums[i] 修改为 nums[i] / C
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> buildArray(vector<int>& nums) 
    {
        int n = nums.size(), C = 1000;
        for (int i = 0; i < n; i++) nums[i] += C * (nums[nums[i]] % C);
        for (int i = 0; i < n; i++) nums[i] /= C;
        return nums;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] buildArray(int[] nums) {
        int n = nums.length, result[] = new int[n];
        for (int i = 0; i < n; i++) result[i] = nums[nums[i]];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def buildArray(self, nums: List[int]) -> List[int]:
        return [nums[nums[i]] for i in range(len(nums))]
```
