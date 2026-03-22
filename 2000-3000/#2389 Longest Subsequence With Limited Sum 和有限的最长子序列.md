# 2389 Longest Subsequence With Limited Sum 和有限的最长子序列

__Description:__

You are given an integer array `nums` of length `n`, and an integer array `queries` of length `m`.

Return _an array_ `answer` _of length_ `m` _where_ `answer[i]` _is the __maximum__ size of a __subsequence__ that you can take from_ `nums` _such that the __sum__ of its elements is less than or equal to_ `queries[i]`.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

```text
Input: nums = [4,5,2,1], queries = [3,10,21]
Output: [2,3,4]
Explanation: We answer the queries as follows:
```

- The subsequence [2,1] has a sum less than or equal to 3. It can be proven that 2 is the maximum size of such a subsequence, so answer[0] = 2.
- The subsequence [4,5,1] has a sum less than or equal to 10. It can be proven that 3 is the maximum size of such a subsequence, so answer[1] = 3.
- The subsequence [4,5,2,1] has a sum less than or equal to 21. It can be proven that 4 is the maximum size of such a subsequence, so answer[2] = 4.

Example 2:

```text
Input: nums = [2,3,4,5], queries = [1]
Output: [0]
Explanation: The empty subsequence is the only subsequence that has a sum less than or equal to 1, so answer[0] = 0.
```

__Constraints:__

- `n == nums.length`
- `m == queries.length`
- `1 <= n, m <= 1000`
- `1 <= nums[i], queries[i] <= 10 ^ 6`

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums` ，和一个长度为 `m` 的整数数组 `queries` 。

返回一个长度为 `m` 的数组 `answer` ，其中 `answer[i]` 是 `nums` 中元素之和小于等于 `queries[i]` 的 __子序列__ 的 __最大__ 长度。

子序列 是由一个数组删除某些元素（也可以不删除）但不改变剩余元素顺序得到的一个数组。

__示例:__

示例 1：

```text
输入：nums = [4,5,2,1], queries = [3,10,21]
输出：[2,3,4]
解释：queries 对应的 answer 如下：
```

- 子序列 [2,1] 的和小于或等于 3 。可以证明满足题目要求的子序列的最大长度是 2 ，所以 answer[0] = 2 。
- 子序列 [4,5,1] 的和小于或等于 10 。可以证明满足题目要求的子序列的最大长度是 3 ，所以 answer[1] = 3 。
- 子序列 [4,5,2,1] 的和小于或等于 21 。可以证明满足题目要求的子序列的最大长度是 4 ，所以 answer[2] = 4 。

示例 2：

```text
输入：nums = [2,3,4,5], queries = [1]
输出：[0]
解释：空子序列是唯一一个满足元素和小于或等于 1 的子序列，所以 answer[0] = 0 。
```

__提示：__

- `n == nums.length`
- `m == queries.length`
- `1 <= n, m <= 1000`
- `1 <= nums[i], queries[i] <= 10 ^ 6`

__思路:__

```text
前缀和 ➕ 二分查找
先将 nums 排序
再计算前缀和
对于每个查询 q, 二分查找前缀和数组中第一个大于 q 的元素的位置
超过前缀和数组的取 n
时间复杂度为 O((M + N)logN), 空间复杂度为 O(1), 可以将前缀和记录在 nums 中, 结果可以记录在 queries 中
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> answerQueries(vector<int>& nums, vector<int>& queries) 
    {
        sort(nums.begin(), nums.end());
        partial_sum(nums.begin(), nums.end(), nums.begin());
        for (int& q : queries) q = ranges::upper_bound(nums, q) - nums.begin();
        return queries;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] answerQueries(int[] nums, int[] queries) {
        int n = nums.length, m = queries.length, pre[] = new int[n + 1], result[] = new int[m];
        Arrays.sort(nums);
        for (int i = 1; i <= n; i++) pre[i] = pre[i - 1] + nums[i - 1];
        for (int i = 0, pos = 0; i < m; i++) result[i] = (pos = Arrays.binarySearch(pre, queries[i])) > 0 ? (pre[pos] == queries[i] ? pos : pos - 1) : (pre[-pos - 2] >= queries[i] ? -pos - 3 : -pos - 2);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def answerQueries(self, nums: List[int], queries: List[int]) -> List[int]:
        print(list(accumulate(sorted(nums))))
        return [bisect_right(nums, q) for q in queries] if (nums := list(accumulate(sorted(nums)))) else [0] * len(queries)
```
