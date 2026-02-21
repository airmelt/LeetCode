# 2913 Subarrays Distinct Element Sum of Squares I 子数组不同元素数目的平方和 I

__Description:__

You are given a __0-indexed__ integer array `nums`.

The __distinct count__ of a subarray of `nums` is defined as:

- Let `nums[i..j]` be a subarray of `nums` consisting of all the indices from `i` to `j` such that `0 <= i <= j < nums.length`. Then the number of distinct values in `nums[i..j]` is called the distinct count of `nums[i..j]`.

Return _the sum of the __squares__ of __distinct counts__ of all subarrays of_ `nums`.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,2,1]
Output: 15
Explanation: Six possible subarrays are:
[1]: 1 distinct value
[2]: 1 distinct value
[1]: 1 distinct value
[1,2]: 2 distinct values
[2,1]: 2 distinct values
[1,2,1]: 2 distinct values
The sum of the squares of the distinct counts in all subarrays is equal to 12 + 12 + 12 + 22 + 22 + 22 = 15.
```

Example 2:

```text
Input: nums = [1,1]
Output: 3
Explanation: Three possible subarrays are:
[1]: 1 distinct value
[1]: 1 distinct value
[1,1]: 1 distinct value
The sum of the squares of the distinct counts in all subarrays is equal to 12 + 12 + 12 = 3.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。

定义 `nums` 一个子数组的 __不同计数__ 值如下:

- 令 `nums[i..j]` 表示 `nums` 中所有下标在 `i` 到 `j` 范围内的元素构成的子数组（满足 `0 <= i <= j < nums.length` ），那么我们称子数组 `nums[i..j]` 中不同值的数目为 `nums[i..j]` 的不同计数。

请你返回 `nums` 中所有子数组的 __不同计数__ 的 __平方__ 和。

由于答案可能会很大，请你将它对 `10 ^ 9 + 7` __取余__ 后返回。

子数组指的是一个数组里面一段连续 非空 的元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,2,1]
输出：15
解释：六个子数组分别为：
[1]: 1 个互不相同的元素。
[2]: 1 个互不相同的元素。
[1]: 1 个互不相同的元素。
[1,2]: 2 个互不相同的元素。
[2,1]: 2 个互不相同的元素。
[1,2,1]: 2 个互不相同的元素。
所有不同计数的平方和为 12 + 12 + 12 + 22 + 22 + 22 = 15 。
```

示例 2：

```text
输入：nums = [2,2]
输出：3
解释：三个子数组分别为：
[2]: 1 个互不相同的元素。
[2]: 1 个互不相同的元素。
[2,2]: 1 个互不相同的元素。
所有不同计数的平方和为 12 + 12 + 12 = 3 。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__思路:__

```text
暴力法
由于数组大小仅有 100
考虑使用暴力法
枚举所有子数组
存入哈希集合
累计哈希集合大小的平方即可
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumCounts(vector<int>& nums) 
    {
        int n = nums.size(), MOD = 1e9 + 7, cur = 0;
        long long result = 0LL;
        for (int i = 0; i < n; i++) 
        {
            unordered_set<int> visited;
            for (int j = i; j < n; j++) 
            {
                visited.insert(nums[j]);
                result = (result + 1LL * (cur = visited.size()) * cur) % MOD;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumCounts(List<Integer> nums) {
        int n = nums.size(), MOD = 1_000_000_007, cur = 0;
        long result = 0L;
        for (int i = 0; i < n; i++) {
            var visited = new HashSet<Integer>();
            for (int j = i; j < n; j++) 
            {
                visited.add(nums.get(j));
                result = (result + 1L * (cur = visited.size()) * cur) % MOD;
            }
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumCounts(self, nums: List[int]) -> int:
        return sum(len(set(nums[i:j + 1])) ** 2 for i in range(len(nums)) for j in range(i, len(nums))) % (10 ** 9 + 7)
```
