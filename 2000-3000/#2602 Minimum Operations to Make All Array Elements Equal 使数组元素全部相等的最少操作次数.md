# 2602 Minimum Operations to Make All Array Elements Equal 使数组元素全部相等的最少操作次数

__Description:__

You are given an array `nums` consisting of positive integers.

You are also given an integer array `queries` of size `m`. For the `i ^ th` query, you want to make all of the elements of `nums` equal to `queries[i]`. You can perform the following operation on the array __any__ number of times:

- __Increase__ or __decrease__ an element of the array by `1`.

Return _an array_ `answer` _of size_ `m` _where_ `answer[i]` _is the __minimum__ number of operations to make all elements of_ `nums` _equal to_ `queries[i]`.

Note that after each query the array is reset to its original state.

__Example:__

Example 1:

```text
Input: nums = [3,1,6,8], queries = [1,5]
Output: [14,10]
Explanation: For the first query we can do the following operations:
```

- Decrease nums[0] 2 times, so that nums = [1,1,6,8].
- Decrease nums[2] 5 times, so that nums = [1,1,1,8].
- Decrease nums[3] 7 times, so that nums = [1,1,1,1].

So the total number of operations for the first query is 2 + 5 + 7 = 14.

For the second query we can do the following operations:

- Increase nums[0] 2 times, so that nums = [5,1,6,8].
- Increase nums[1] 4 times, so that nums = [5,5,6,8].
- Decrease nums[2] 1 time, so that nums = [5,5,5,8].
- Decrease nums[3] 3 times, so that nums = [5,5,5,5].

So the total number of operations for the second query is 2 + 4 + 1 + 3 = 10.

Example 2:

```text
Input: nums = [2,9,6,3], queries = [10]
Output: [20]
Explanation: We can increase each value in the array to 10. The total number of operations will be 8 + 1 + 4 + 7 = 20.
```

__Constraints:__

- `n == nums.length`
- `m == queries.length`
- `1 <= n, m <= 10 ^ 5`
- `1 <= nums[i], queries[i] <= 10 ^ 9`

__题目描述:__

给你一个正整数数组 `nums` 。

同时给你一个长度为 `m` 的整数数组 `queries` 。第 `i` 个查询中，你需要将 `nums` 中所有元素变成 `queries[i]` 。你可以执行以下操作 __任意__ 次:

- 将数组里一个元素 __增大__ 或者 __减小__ `1` 。

请你返回一个长度为 `m` 的数组 `answer` ，其中 `answer[i]`是将 `nums` 中所有元素变成 `queries[i]` 的 __最少__ 操作次数。

注意，每次查询后，数组变回最开始的值。

__示例:__

示例 1：

```text
输入：nums = [3,1,6,8], queries = [1,5]
输出：[14,10]
解释：第一个查询，我们可以执行以下操作：
```

- 将 nums[0] 减小 2 次，nums = [1,1,6,8] 。
- 将 nums[2] 减小 5 次，nums = [1,1,1,8] 。
- 将 nums[3] 减小 7 次，nums = [1,1,1,1] 。

第一个查询的总操作次数为 2 + 5 + 7 = 14 。

第二个查询，我们可以执行以下操作：

- 将 nums[0] 增大 2 次，nums = [5,1,6,8] 。
- 将 nums[1] 增大 4 次，nums = [5,5,6,8] 。
- 将 nums[2] 减小 1 次，nums = [5,5,5,8] 。
- 将 nums[3] 减小 3 次，nums = [5,5,5,5] 。

第二个查询的总操作次数为 2 + 4 + 1 + 3 = 10 。

示例 2：

```text
输入：nums = [2,9,6,3], queries = [10]
输出：[20]
解释：我们可以将数组中所有元素都增大到 10 ，总操作次数为 8 + 1 + 4 + 7 = 20 。
```

__提示：__

- `n == nums.length`
- `m == queries.length`
- `1 <= n, m <= 10 ^ 5`
- `1 <= nums[i], queries[i] <= 10 ^ 9`

__思路:__

```text
前缀和 ➕ 二分
由于 nums 最终要变成 queries[i], 因此可以先对 nums 进行排序, 顺序不影响结果
然后对 nums 进行前缀和处理
对于每个查询 queries[i], 可以使用二分查找找到 nums 中第一个大于等于 queries[i] 的位置 pos
对于不大于 queries[i] 的元素, 需要将其增大到 queries[i], 对应的操作次数为 queries[i] * pos - pre[pos]
对于大于 queries[i] 的元素, 需要将其减小到 queries[i], 对应的操作次数为 pre[n] - pre[pos] - queries[i] * (n - pos)
因此对于每个查询, 最终的操作次数为 queries[i] * pos - pre[pos] + pre[n] - pre[pos] - queries[i] * (n - pos)
时间复杂度为 O((M + N)logN), 空间复杂度为 O(N), M 为 queries 的长度, N 为 nums 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> minOperations(vector<int>& nums, vector<int>& queries) 
    {
        sort(nums.begin(), nums.end());
        int n = nums.size(), pos = 0, m = queries.size();
        vector<long long> pre(n + 1), result;
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        for (const auto& q : queries) result.emplace_back((long long)q * (pos = ranges::lower_bound(nums, q) - nums.begin()) - pre[pos] + pre[n] - pre[pos] - (long long)q * (n - pos));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Long> minOperations(int[] nums, int[] queries) {
        Arrays.sort(nums);
        int n = nums.length, pos = 0;
        long[] pre = new long[n + 1];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        List<Long> result = new ArrayList<>();
        for (int q : queries) result.add((long)q * (pos = helper(nums, q)) - pre[pos] + pre[n] - pre[pos] - (long)q * (n - pos));
        return result;
    }

    private int helper(int[] nums, int target) {
        int left = 0, right = nums.length - 1, mid = 0;
        while (left <= right) {
            if (nums[mid = left + ((right - left) >>> 1)] < target) left = mid + 1;
            else right = mid - 1;
        }
        return left;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int], queries: List[int]) -> List[int]:
        nums.sort()
        pre, n = [0] + list(accumulate(nums)), len(nums)
        return [] if not (n := len(nums)) or not (pre := [0] + list(accumulate(nums))) else [q * (pos := bisect_left(nums, q)) - pre[pos] + (pre[-1] - pre[pos]) - q * (n - pos) for q in queries]
```
