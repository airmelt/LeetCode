# 3080 Mark Elements on Array by Performing Queries 执行操作标记数组中的元素

__Description:__

You are given a __0-indexed__ array `nums` of size `n` consisting of positive integers.

You are also given a 2D array `queries` of size `m` where `queries[i] = [index_i, k_i]`.

Initially all elements of the array are unmarked.

You need to apply `m` queries on the array in order, where on the `i ^ th` query you do the following:

- Mark the element at index `index_i` if it is not already marked.
- Then mark `k_i` unmarked elements in the array with the __smallest__ values. If multiple such elements exist, mark the ones with the smallest indices. And if less than `k_i` unmarked elements exist, then mark all of them.

Return _an array answer of size_ `m` _where_ `answer[i]` _is the __sum__ of unmarked elements in the array after the_ `i ^ th` _query_.

__Example:__

Example 1:

```text
Input: nums = [1,2,2,1,2,3,1], queries = [[1,2],[3,3],[4,2]]

Output: [8,3,0]

Explanation:

We do the following queries on the array:
```

- Mark the element at index `1`, and `2` of the smallest unmarked elements with the smallest indices if they exist, the marked elements now are nums = [__1__,__2__,2,__1__,2,3,1]. The sum of unmarked elements is `2 + 2 + 3 + 1 = 8`.
- Mark the element at index `3`, since it is already marked we sk_ip it. Then we mark `3` of the smallest unmarked elements with the smallest indices, the marked elements now are nums = [__1__,__2__,__2__,__1__,__2__,3,__1__]. The sum of unmarked elements is `3`.
- Mark the element at index `4`, since it is already marked we sk_ip it. Then we mark `2` of the smallest unmarked elements with the smallest indices if they exist, the marked elements now are nums = [__1__,__2__,__2__,__1__,__2__,__3__,__1__]. The sum of unmarked elements is `0`.

Example 2:

```text
Input: nums = [1,4,2,3], queries = [[0,1]]

Output: [7]
```

__Explanation:__ We do one query which is mark the element at index `0` and mark the smallest element among unmarked elements. The marked elements will be nums = [__1__,4,__2__,3], and the sum of unmarked elements is `4 + 3 = 7`.

__Constraints:__

- `n == nums.length`
- `m == queries.length`
- `1 <= m <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= index_i, k_i <= n - 1`

__题目描述:__

给你一个长度为 `n` 下标从 __0__ 开始的正整数数组 `nums` 。

同时给你一个长度为 `m` 的二维操作数组 `queries` ，其中 `queries[i] = [index_i, k_i]` 。

一开始，数组中的所有元素都 未标记 。

你需要依次对数组执行 `m` 次操作，第 `i` 次操作中，你需要执行:

- 如果下标 `index_i` 对应的元素还没标记，那么标记这个元素。
- 然后标记 `k_i` 个数组中还没有标记的 __最小__ 元素。如果有元素的值相等，那么优先标记它们中下标较小的。如果少于 `k_i` 个未标记元素存在，那么将它们全部标记。

请你返回一个长度为 `m` 的数组 `answer` ，其中 `answer[i]`是第 `i` 次操作后数组中还没标记元素的 __和__ 。

__示例:__

示例 1：

```text
输入：nums = [1,2,2,1,2,3,1], queries = [[1,2],[3,3],[4,2]]

输出：[8,3,0]

解释：

我们依次对数组做以下操作：
```

- 标记下标为 `1` 的元素，同时标记 `2` 个未标记的最小元素。标记完后数组为 nums = [___1___,___2___,2,___1___,2,3,1] 。未标记元素的和为 `2 + 2 + 3 + 1 = 8` 。
- 标记下标为 `3` 的元素，由于它已经被标记过了，所以我们忽略这次标记，同时标记最靠前的 `3` 个未标记的最小元素。标记完后数组为 nums = [___1___,___2___,___2___,___1___,___2___,3,___1___]` 。未标记元素的和为 `3` 。
- 标记下标为 `4` 的元素，由于它已经被标记过了，所以我们忽略这次标记，同时标记最靠前的 `2` 个未标记的最小元素。标记完后数组为 nums = [___1___,___2___,___2___,___1___,___2___,___3___,___1___] 。未标记元素的和为 `0` 。

示例 2：

```text
输入：nums = [1,4,2,3], queries = [[0,1]]

输出：[7]
```

__解释:__ 我们执行一次操作，将下标为 `0` 处的元素标记，并且标记最靠前的 `1` 个未标记的最小元素。标记完后数组为 nums = [___1___,4,___2___,3] 。未标记元素的和为 `4 + 3 = 7` 。

__提示：__

- `n == nums.length`
- `m == queries.length`
- `1 <= m <= n <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= index_i, k_i <= n - 1`

__思路:__

```text
1. 优先队列
将数组下标和元素同时传入优先队列(最小堆)
记录数组的和 s = sum(nums)
先尝试标记
如果没标记就标记, 将对应下标加入哈希表, 并从 s 中减去相应值
每次弹出堆顶
检查是否标记, 以及标记总数是否超过数组大小
如果都没有就标记
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
2. 排序
使用稳定排序对下标进行排序
按照顺序标记
标记可以将数组元素直接改为 0, 因为数组元素均大于 0
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> unmarkedSumArray(vector<int>& nums, vector<vector<int>>& queries) 
    {
        int n = nums.size(), m = queries.size(), i = 0, j = 0, k = 0;
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        stable_sort(idx.begin(), idx.end(), [&](int a, int b) { return nums[a] < nums[b]; });
        long long s = accumulate(nums.begin(), nums.end(), 0LL);
        vector<long long> result(m);
        for (int qi = 0; qi < m; qi++) 
        {
            s -= nums[i = queries[qi][0]];
            nums[i] = 0;
            for (k = queries[qi][1]; j < n and k; j++) 
            {
                if (nums[i = idx[j]]) 
                {
                    s -= nums[i];
                    nums[i] = 0;
                    --k;
                }
            }
            result[qi] = s;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long[] unmarkedSumArray(int[] nums, int[][] queries) {
        int n = nums.length, m = queries.length, i = 0, j = 0, k = 0;
        Integer[] idx = new Integer[n];
        for (i = 0; i < n; i++) idx[i] = i;
        Arrays.sort(idx, (a, b) -> nums[a] - nums[b]);
        long s = Arrays.stream(nums).mapToLong(v -> v).sum(), result[] = new long[m];
        for (int qi = 0; qi < m; qi++) {
            s -= nums[i = queries[qi][0]];
            nums[i] = 0;
            for (k = queries[qi][1]; j < n && k > 0; j++) {
                if (nums[i = idx[j]] > 0) {
                    s -= nums[i];
                    nums[i] = 0;
                    --k;
                }
            }
            result[qi] = s;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def unmarkedSumArray(self, nums: List[int], queries: List[List[int]]) -> List[int]:
        s, visited, heap, n, result = sum(nums), set(), [(num, i) for i, num in enumerate(nums)], len(nums), []
        heapify(heap)
        for idx, k in queries:
            if idx not in visited:
                visited.add(idx)
                s -= nums[idx]
            while k and len(visited) < n:
                num, i = heappop(heap)
                if i not in visited:
                    visited.add(i)
                    k -= 1
                    s -= num
            result.append(s)
        return result
```
