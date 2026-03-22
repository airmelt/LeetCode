# 1906 Minimum Absolute Difference Queries 查询差绝对值的最小值

__Description:__

The __minimum absolute difference__ of an array `a` is defined as the __minimum value__ of `|a[i] - a[j]|`, where `0 <= i < j < a.length` and `a[i] != a[j]`. If all elements of `a` are the __same__, the minimum absolute difference is `-1`.

- For example, the minimum absolute difference of the array `[5,2,3,7,2]` is `|2 - 3| = 1`. Note that it is not `0` because `a[i]` and `a[j]` must be different.

You are given an integer array `nums` and the array `queries` where `queries[i] = [li, ri]`. For each query `i`, compute the __minimum absolute difference__ of the __subarray__ `nums[li...ri]` containing the elements of `nums` between the __0-based__ indices `li` and `ri` (__inclusive__).

Return _an __array___ `ans` _where_ `ans[i]` _is the answer to the_ `i ^ th` _query_.

A subarray is a contiguous sequence of elements in an array.

The value of `|x|` is defined as:

- `x` if `x >= 0`.
- `-x` if `x < 0`.

__Example:__

Example 1:

```text
Input: nums = [1,3,4,8], queries = [[0,1],[1,2],[2,3],[0,3]]
Output: [2,1,4,1]
Explanation: The queries are processed as follows:
- queries[0] = [0,1]: The subarray is [1,3] and the minimum absolute difference is |1-3| = 2.
- queries[1] = [1,2]: The subarray is [3,4] and the minimum absolute difference is |3-4| = 1.
- queries[2] = [2,3]: The subarray is [4,8] and the minimum absolute difference is |4-8| = 4.
- queries[3] = [0,3]: The subarray is [1,3,4,8] and the minimum absolute difference is |3-4| = 1.
```

Example 2:

```text
Input: nums = [4,5,2,2,7,10], queries = [[2,3],[0,2],[0,5],[3,5]]
Output: [-1,1,1,3]
Explanation: The queries are processed as follows:
- queries[0] = [2,3]: The subarray is [2,2] and the minimum absolute difference is -1 because all the
  elements are the same.
- queries[1] = [0,2]: The subarray is [4,5,2] and the minimum absolute difference is |4-5| = 1.
- queries[2] = [0,5]: The subarray is [4,5,2,2,7,10] and the minimum absolute difference is |4-5| = 1.
- queries[3] = [3,5]: The subarray is [2,7,10] and the minimum absolute difference is |7-10| = 3.
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 100`
- `1 <= queries.length <= 2 * 10 ^ 4`
- `0 <= li < ri < nums.length`

__题目描述:__

一个数组 `a` 的 __差绝对值的最小值__ 定义为: `0 <= i < j < a.length` 且 `a[i] != a[j]` 的 `|a[i] - a[j]|` 的 __最小值__。如果 `a` 中所有元素都 __相同__ ，那么差绝对值的最小值为 `-1` 。

- 比方说，数组 `[5,2,3,7,2]` 差绝对值的最小值是 `|2 - 3| = 1` 。注意答案不为 `0` ，因为 `a[i]` 和 `a[j]` 必须不相等。

给你一个整数数组 `nums` 和查询数组 `queries` ，其中 `queries[i] = [li, ri]` 。对于每个查询 `i` ，计算 __子数组__ `nums[li...ri]` 中 __差绝对值的最小值__ ，子数组 `nums[li...ri]` 包含 `nums` 数组（下标从 __0__ 开始）中下标在 `li` 和 `ri` 之间的所有元素（包含 `li` 和 `ri` 在内）。

请你返回 `ans` __数组__，其中 `ans[i]` 是第 `i` 个查询的答案。

子数组 是一个数组中连续的一段元素。

`|x|` 的值定义为:

- 如果 `x >= 0` ，那么值为 `x` 。
- 如果 `x < 0` ，那么值为 `-x` 。

__示例:__

示例 1：

```text
输入：nums = [1,3,4,8], queries = [[0,1],[1,2],[2,3],[0,3]]
输出：[2,1,4,1]
解释：查询结果如下：
- queries[0] = [0,1]：子数组是 [1,3] ，差绝对值的最小值为 |1-3| = 2 。
- queries[1] = [1,2]：子数组是 [3,4] ，差绝对值的最小值为 |3-4| = 1 。
- queries[2] = [2,3]：子数组是 [4,8] ，差绝对值的最小值为 |4-8| = 4 。
- queries[3] = [0,3]：子数组是 [1,3,4,8] ，差的绝对值的最小值为 |3-4| = 1 。
```

示例 2：

```text
输入：nums = [4,5,2,2,7,10], queries = [[2,3],[0,2],[0,5],[3,5]]
输出：[-1,1,1,3]
解释：查询结果如下：
- queries[0] = [2,3]：子数组是 [2,2] ，差绝对值的最小值为 -1 ，因为所有元素相等。
- queries[1] = [0,2]：子数组是 [4,5,2] ，差绝对值的最小值为 |4-5| = 1 。
- queries[2] = [0,5]：子数组是 [4,5,2,2,7,10] ，差绝对值的最小值为 |4-5| = 1 。
- queries[3] = [3,5]：子数组是 [2,7,10] ，差绝对值的最小值为 |7-10| = 3 。
```

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 100`
- `1 <= queries.length <= 2 * 10 ^ 4`
- `0 <= li < ri < nums.length`

__思路:__

```text
前缀和
注意到 nums[i] 的范围仅为 [1, 100]
如果 nums[i] 已经出现, pre[i][nums[i]] 就自增 1
这样 pre[i][j] 表示 nums[:i] 中 j 出现的次数
pre[i] 有序, 只需要比较相邻的两个 pre[i][j] 即可
每次遍历查询中的每一个数字求出最小值即可
时间复杂度为 O((N + M)C), 空间复杂度为 O(NC), 其中 N 为 nums 的长度, M 为 queries 的长度, C 为 nums 中数字的范围, 本题中 C = 101
```

__代码:__

__C++__:

```C++
class Solution {
public:
    vector<int> minDifference(vector<int>& nums, vector<vector<int>>& queries) {
        int C = 101, m = nums.size(), n = queries.size();
        vector<vector<int>> pre(m + 1, vector<int>(C));
        vector<int> result(n);
        for (int i = 0; i < m; i++) 
        {
            copy_n(pre[i].begin(), C, pre[i + 1].begin());
            ++pre[i + 1][nums[i]];
        }
        for (int i = 0; i < n; i++) 
        {
            int left = queries[i].front(), right = queries[i].back(), last = 0, best = C;
            for (int j = 1; j < C; j++) 
            {
                if (pre[left][j] != pre[right + 1][j]) 
                {
                    if (last) best = min(best, j - last);
                    last = j;
                }
            }
            result[i] = best == C ? -1 : best;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] minDifference(int[] nums, int[][] queries) {
        int C = 101, m = nums.length, n = queries.length, pre[][] = new int[m + 1][C], result[] = new int[n];
        for (int i = 0; i < m; i++) {
            pre[i + 1] = Arrays.copyOf(pre[i], C);
            ++pre[i + 1][nums[i]];
        }
        for (int i = 0; i < n; i++) {
            int left = queries[i][0], right = queries[i][1], last = 0, best = C;
            for (int j = 1; j < C; j++) {
                if (pre[left][j] != pre[right + 1][j]) {
                    if (last != 0) best = Math.min(best, j - last);
                    last = j;
                }
            }
            result[i] = best == C ? -1 : best;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minDifference(self, nums: List[int], queries: List[List[int]]) -> List[int]:
        pre, n, result = [[0] * (C := 101)], len(nums), []
        for i, num in enumerate(nums):
            pre.append(pre[-1][:])
            pre[-1][num] += 1
        for left, right in queries:
            last, best = 0, C
            for j in range(1, C):
                if pre[left][j] != pre[right + 1][j]:
                    if last:
                        best = min(best, j - last)
                    last = j
            result.append(-1 if best == C else best)
        return result
```
