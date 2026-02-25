# 2916 Subarrays Distinct Element Sum of Squares II 子数组不同元素数目的平方和 II

__Description:__

You are given a __0-indexed__ integer array `nums`.

The __distinct count__ of a subarray of `nums` is defined as:

- Let `nums[i..j]` be a subarray of `nums` consisting of all the indices from `i` to `j` such that `0 <= i <= j < nums.length`. Then the number of distinct values in `nums[i..j]` is called the distinct count of `nums[i..j]`.

Return _the sum of the __squares__ of __distinct counts__ of all subarrays of_ `nums`.

Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

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
Input: nums = [2,2]
Output: 3
Explanation: Three possible subarrays are:
[2]: 1 distinct value
[2]: 1 distinct value
[2,2]: 1 distinct value
The sum of the squares of the distinct counts in all subarrays is equal to 12 + 12 + 12 = 3.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

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

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
线段树
需要计算当前 nums[i] 到上一个相同值位置的子数组的变化情况
如果子数组本来是 x 个不同计数, 其平方为 x ^ 2
那么如果增加一个不同计数, 增加的数量为 (x + 1) ^ 2 - x ^ 2 = x * 2 + 1
记录每个元素出现的位置 j, 默认为 0, 位置使用哈希表存储
将区间 [j + 1, i] 都加上 1, 这里可以用线段树加快处理
同时把 2 * query(1, 1, n, i, j) + i - j 加入到累计量中, 这一部分即不同计数的变化量
把累计量加入到结果中
将 nums[i] 的下标更新
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumCounts(vector<int>& nums) 
    {
        int n = nums.size();
        tree.resize(n << 2);
        lazy.resize(n << 2);
        unordered_map<int, int> last;
        long long result = 0LL, cur = 0LL, MOD = 1e9 + 7LL;
        for (int i = 1, x = 0, j = 0; i <= n; i++)
        {
            cur += (query_and_update(1, 1, n, (j = last[x = nums[i - 1]]) + 1, i) << 1) + i - j;
            result = (result + cur) % MOD;
            last[x] = i;
        }
        return result;
    }
private:
    vector<long long> tree;
    vector<int> lazy;

    long long query_and_update(int p, int l, int r, int s, int t)
    {
        long long result = 0LL;
        if (s <= l and r <= t)
        {
            result = tree[p];
            update(p, l, r, 1);
            return result;
        }
        int m = l + ((r - l) >> 1), v = lazy[p];
        if (v)
        {
            update(p << 1, l, m, v);
            update((p << 1) + 1, m + 1, r, v);
            lazy[p] = 0;
        }
        if (s <= m) result += query_and_update(p << 1, l, m, s, t);
        if (m < t) result += query_and_update((p << 1) + 1, m + 1, r, s, t);
        tree[p] = tree[p << 1] + tree[(p << 1) + 1];
        return result;
    }

    void update(int p, int l, int r, int v)
    {
        tree[p] += (long long)v * (r - l + 1);
        lazy[p] += v;
    }
};
```

__Java__:

```Java
class Solution {
    private long[] tree;
    private int[] lazy;

    public int sumCounts(int[] nums) {
        int n = nums.length;
        tree = new long[n << 2];
        lazy = new int[n << 2];
        long result = 0L, cur = 0L, MOD = 1_000_000_007L;
        var last = new HashMap<Integer, Integer>();
        for (int i = 1, x = 0, j = 0; i <= n; i++) {
            cur += (queryAndAdd(1, 1, n, (j = last.getOrDefault(x = nums[i - 1], 0)) + 1, i) << 1) + i - j;
            result = (result + cur) % MOD;
            last.put(x, i);
        }
        return (int)result;
    }

    private long queryAndAdd(int p, int l, int r, int s, int t) {
        long result = 0L;
        if (s <= l && r <= t) {
            result = tree[p];
            update(p, l, r, 1);
            return result;
        }
        int m = l + ((r - l) >>> 1), v = lazy[p];
        if (v != 0) {
            update(p << 1, l, m, v);
            update((p << 1) + 1, m + 1, r, v);
            lazy[p] = 0;
        }
        if (s <= m) result += queryAndAdd(p << 1, l, m, s, t);
        if (m < t) result += queryAndAdd((p << 1) + 1, m + 1, r, s, t); 
        tree[p] = tree[p << 1] + tree[(p << 1) + 1];
        return result;
    }

    private void update(int p, int l, int r, int v) {
        tree[p] += (long)v * (r - l + 1);
        lazy[p] += v;
    }
}
```

__Python__:

```Python
class Solution:
    def sumCounts(self, nums: List[int]) -> int:
        tree, lazy, result, cur, last, MOD = [0] * ((n := len(nums)) << 2), [0] * (n << 2), 0, 0, {}, 10 ** 9 + 7

        def query_and_add(p: int, l: int, r: int, s: int, t: int) -> int:
            if s <= l and r <= t:
                result = tree[p]
                update(p, l, r, 1)
                return result
            m, result = l + ((r - l) >> 1), 0
            if lazy[p]:
                update(p << 1, l, m, lazy[p])
                update((p << 1) + 1, m + 1, r, lazy[p])
                lazy[p] = 0
            if s <= m:
                result += query_and_add(p << 1, l, m, s, t)
            if m < t:
                result += query_and_add((p << 1) + 1, m + 1, r, s, t)
            tree[p] = tree[p << 1] + tree[(p << 1) + 1]
            return result

        def update(p: int, l: int, r: int, v: int) -> None:
            tree[p] += v * (r - l + 1)
            lazy[p] += v

        for i, num in enumerate(nums, 1):
            cur += (query_and_add(1, 1, n, (j := last.get(num, 0)) + 1, i) << 1) + i - j
            result += cur
            last[num] = i
        return result % MOD
```
